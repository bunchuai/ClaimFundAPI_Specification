# ClaimFundAPI — Frontend Integration Spec

**Version:** v1.0.0 · OpenAPI 3.0.1
**Base URL (DEV tunnel):** `https://s9ps9j5t-5001.asse.devtunnels.ms`
**ขอบเขต:** บริการกลางสำหรับโอนเงินจ่ายค่าสินไหมทดแทนของการเคลมประกันภัย — สร้างรายการโอน ตรวจสอบบัญชีผู้รับ ติดตามธุรกรรม และจัดการสถานะการชำระเงิน

---

## 1. Authentication

OAuth2 — Authorization Code flow

| รายการ | ค่า |
|---|---|
| Authorization URL | `https://authlogin.uatsiamsmile.com/connect/authorize` |
| Token URL | `https://authlogin.uatsiamsmile.com/connect/token` |
| Scopes | `ClaimFundAPI`, `openid`, `profile`, `roles` |

ทุก endpoint (ยกเว้น `GetApplicationVersionControl`) ต้องส่ง header:

```
Authorization: Bearer <access_token>
```

**Error ที่ต้อง handle:** `401 Unauthorized` (token หมดอายุ/ไม่มี → redirect ไป login หรือ refresh), `403 Forbidden` (ไม่มีสิทธิ์ → แสดงหน้า no-permission)

> แนะนำให้ทำ axios/fetch interceptor กลาง: แนบ token อัตโนมัติ + ดัก 401 เพื่อ refresh แล้ว retry 1 ครั้ง

---

## 2. Response Envelope (สำคัญมาก)

**ทุก endpoint คืนค่าในรูปแบบ envelope เดียวกัน** ต่างกันแค่ชนิดของ `data` (object หรือ array)

```ts
interface ServiceResponse<T> {
  data: T | null;
  isSuccess: boolean;
  message: string | null;
  code: number | null;
  exceptionMessage: unknown | null;
  serverDateTime: string;            // ISO 8601
  totalAmountRecords: number | null; // สำหรับ paging (ยังไม่มี endpoint ไหนรับ param paging)
  totalAmountPages: number | null;
  currentPage: number | null;
  recordsPerPage: number | null;
  pageIndex: number | null;
}
```

**กติกาฝั่ง frontend:**
1. เช็ค HTTP status ก่อน → แล้วเช็ค `isSuccess` เสมอ (HTTP 200 + `isSuccess: false` เป็นไปได้)
2. แสดง `message` เป็นข้อความ error ให้ user, ส่ง `exceptionMessage` เข้า log เท่านั้น (ห้ามแสดงบน UI)
3. `data` เป็น nullable ทุกกรณี — ต้อง guard ก่อนใช้
4. บาง DTO มี `isSuccess`/`message` ของตัวเองซ้อนอยู่ใน `data` (เช่น `AddHospitalSettingResponseDto`) — **ต้องเช็คสองชั้น**

```ts
// helper ที่แนะนำ
async function callApi<T>(...): Promise<T> {
  const res = await http(...);
  if (!res.data.isSuccess) throw new ApiError(res.data.message, res.data.code);
  return res.data.data as T;
}
```

**Content-Type:** ส่ง `application/json` เสมอ (API รับ `application/json-patch+json`, `text/json`, `application/*+json` ด้วย แต่ไม่จำเป็นต้องใช้)

---

## 3. สรุป Endpoint ทั้งหมด (21 รายการ)

### 3.1 HospitalPaymentSetting — ตั้งค่าการจ่ายเงินรายสถานพยาบาล

| Method | Path | หน้าที่ |
|---|---|---|
| GET | `/api/HospitalPaymentSetting/Finance/SearchHospital` | ค้นหาชื่อสถานพยาบาล (autocomplete) |
| GET | `/api/HospitalPaymentSetting/Finance/HospitalSettingMonitor` | รายการตั้งค่าการจ่ายเงินทั้งหมด |
| POST | `/api/HospitalPaymentSetting/Finance/AddHospitalSetting` | เพิ่มสถานพยาบาลที่ต้องการตั้งค่า |
| POST | `/api/HospitalPaymentSetting/Finance/UpdateHospitalSetting` | แก้ไขการตั้งค่า |
| GET | `/api/HospitalPaymentSetting/Finance/GetHistories` | ประวัติการตั้งค่า |

**SearchHospital**
- Query: `orgName` *(string, **required**)*
- Response: `ServiceResponse<SearchHospitalResponseDto[]>`
```ts
interface SearchHospitalResponseDto { orgId: number; orgName: string | null; }
```
- UI: ใช้ทำ autocomplete — แนะนำ debounce 300ms, ต้องส่งค่าเสมอ (ว่าง = 400)
- `orgId` ที่ได้ = `hospitalId` ที่ใช้ตอน Add

**HospitalSettingMonitor**
- Query: `searchDetail` *(string, optional)*
- Response: `ServiceResponse<HospitalSettingMonitorReponseDto[]>` *(สะกด "Reponse" ตาม API จริง)*
```ts
interface HospitalSettingMonitorReponseDto {
  hospitalPaymentSettingId: string;   // uuid — ใช้เป็น key และส่งต่อไป Update/GetHistories
  hospitalName: string | null;        // สถานพยาบาล
  isAutoPay: boolean | null;          // จ่ายเงินอัตโนมัติ
  delayDays: number;                  // จำนวนวันที่หน่วงการจ่าย
  holdStatusId: number | null;        // สถานะ Hold
  holdStatusName: string | null;
  updatedDate: string | null;         // ISO date-time
}
```

**AddHospitalSetting** — Body `AddHospitalSettingRequestDto`
```ts
{
  hospitalId: number;
  hospitalName: string;      // required, minLength 1
  autoPayDelayDays: number;
  holdStatusId: number;
  isAutoPay: boolean;
}
```
Response: `ServiceResponse<{ isSuccess: boolean; message: string | null }>`

**UpdateHospitalSetting** — Body `UpdateHospitalSettingRequestDto`
```ts
{
  hospitalPaymentSettingId: string;  // uuid
  autoPayDelayDays: number;
  holdStatusId: number;
  isAutoPay: boolean;
}
```
Response: เหมือน Add
> ⚠️ เป็น **POST** ไม่ใช่ PUT/PATCH

**GetHistories**
- Query: `hospitalPaymentSettingId` *(uuid, optional ตาม spec แต่ควรส่งเสมอ)*
- Response: `ServiceResponse<HistoryResponseDto[]>`
```ts
interface HistoryResponseDto {
  actionName: string | null;
  createdDate: string | null;
  createdByUser: string | null;
}
```

---

### 3.2 Masters — ข้อมูล dropdown / lookup

| Method | Path | ใช้ทำอะไร |
|---|---|---|
| GET | `/api/Masters/GetAdjustmentReasons` | สาเหตุการโอนเพิ่ม (list) |
| GET | `/api/Masters/GetAdjustmentReasonById` | สาเหตุการโอนเพิ่ม by id — query `AdjustmentReasonId` (int, **ขึ้นต้นตัวใหญ่**) |
| GET | `/api/Masters/GetTransferStatuses` | สถานะการโอนเงิน |
| GET | `/api/Masters/GetPaymentStatuses` | สถานะการจ่ายเงิน |
| GET | `/api/Masters/GetBankAccountRelationTypes` | ประเภทความสัมพันธ์ของบัญชีกับผู้รับสินไหม |
| GET | `/api/Masters/GetBankAccountRelationTypeById` | by id — query `bankAccountRelationTypeId` (int) |

ทุกตัวคืน DTO หน้าตาเดียวกัน:
```ts
interface MasterItem { id: number; name: string | null; }
```
→ `ServiceResponse<MasterItem[]>` (แบบ list) หรือ `ServiceResponse<MasterItem>` (by id)

**คำแนะนำ:** ดึง master ทั้งหมดตอน app boot แล้ว cache ไว้ (React Query `staleTime: Infinity` หรือ store กลาง) — ไม่ต้องยิงซ้ำทุกหน้า. ส่วน `*ById` แทบไม่จำเป็นถ้า cache list ไว้แล้ว

> ⚠️ ไม่มี endpoint `GetHoldStatuses` — `holdStatusId` ที่ใช้ในหน้า Hospital Setting น่าจะมาจาก `GetPaymentStatuses` หรือ `GetTransferStatuses` **ต้องยืนยันกับ backend**

---

### 3.3 Transfer — สร้างรายการโอนเงิน

| Method | Path | หน้าที่ |
|---|---|---|
| POST | `/api/Transfer/v1/CreatePayment` | สร้างรายการโอนเงิน และโอนเงิน |
| GET | `/api/Transfer/v1/PaymentDetails` | ดูรายละเอียดรายการโอน |
| POST | `/api/Transfer/v1/CreateTransferResult` | (Step 3) รับผลการโอนจากธนาคาร — **สำหรับทดสอบ/callback ไม่ใช่ของ frontend** |

**CreatePayment** — Body เป็น **Array** ของ `CreatePaymentRequestDto`

```ts
type CreatePaymentBody = CreatePaymentRequestDto[];

interface CreatePaymentRequestDto {
  casePayableId: string;          // uuid  — required
  grossPaidAmount: number;        // required — ยอดก่อนหัก
  withHoldingTaxAmount: number;   // required — ภาษีหัก ณ ที่จ่าย
  netPaidAmount: number;          // required — ยอดโอนสุทธิ
  receivingBankId: number;        // required
  receivingBankAccountNo: string; // required, minLength 1
  receivingBankName: string;      // required, minLength 1
  receivingAccountName: string;   // required, minLength 1
  phoneNumber: string;            // required — ใช้ส่ง SMS แจ้งผู้รับเงิน
  claimCase: string;              // required
  claimNo: string;                // required
  payeeTypeId?: number;           // optional
  paymentTypeId?: number;         // optional
  userId?: number;                // optional
}
```

Response: `ServiceResponse<CreatePaymentResponseDto>`
```ts
interface CreatePaymentResponseDto {
  itemCout: number;                  // (sic) จำนวนรายการที่สร้าง — สะกดผิดตาม API
  totalNetPaidAmount: number | null;
  isSuccess: boolean;                // เช็คซ้ำอีกชั้นนอกเหนือจาก envelope
  message: string | null;
  payListHeaderId: string | null;    // uuid
  refTransactionId: string | null;   // uuid → ใช้เป็น referenceCode ของ PaymentDetails
  paymentCodeResponse: { paymentCode: string | null }[] | null;
}
```

**Validation ที่ frontend ควรทำก่อนส่ง:**
- `netPaidAmount === grossPaidAmount - withHoldingTaxAmount`
- ทุกยอดเงิน > 0, ปัดทศนิยม 2 ตำแหน่ง
- `receivingBankAccountNo` — ตัวเลขล้วน, ตัด `-`/ช่องว่างออกก่อนส่ง
- `phoneNumber` — รูปแบบเบอร์ไทย 10 หลัก
- ฟิลด์ required ห้ามเป็น empty string (`minLength: 1`)

> ⚠️ **เป็น operation ที่โอนเงินจริง** — ต้องมี confirm dialog, disable ปุ่มระหว่างรอ response, และกัน double-submit ให้แน่น (เช่น idempotency ฝั่ง UI + block ปุ่มจนกว่าจะได้คำตอบ) เพราะ API ไม่มี idempotency key

**PaymentDetails**
- Query: `referenceCode` *(uuid, **required**)* — คือ `refTransactionId` จาก CreatePayment
- Response: `ServiceResponse<PaymentTransactionResponseDto>`
```ts
interface PaymentTransactionResponseDto {
  paymentDate: string | null;
  printDate: string | null;
  admissionDate: string | null;
  countItem: number;
  paymentId: string;                 // uuid
  totalNetPaidAmount: number | null;
  customerName: string | null;
  employee: string | null;
  accountNo: string | null;
  accountName: string | null;
  bankName: string | null;
  paymentCode: string | null;
  paymentItemDetail: PaymentItemDetailDto[] | null;
}

interface PaymentItemDetailDto {
  claimNo: string | null;
  customerName: string | null;
  transactionAmount: number | null;
  bankName: string | null;
  approvedAmount: number | null;
}
```
- ใช้ทำหน้า "ใบสรุปการโอน / Payment slip" — `paymentItemDetail` คือรายการย่อยในตาราง

---

### 3.4 Setting — ตั้งค่าโอนเงินอัตโนมัติระดับระบบ

| Method | Path | หน้าที่ |
|---|---|---|
| GET | `/api/Setting/GetCurrentSetting` | อ่านค่าตั้งค่าปัจจุบัน + ประวัติ |
| POST | `/api/Setting/UpdateSettingAutoTransfer` | บันทึกการตั้งค่า |
| GET | `/api/Setting/SearchClaimOrCase` | ค้นหาจาก ClaimNo / CaseNo |

**GetCurrentSetting** → `ServiceResponse<PayTransferSettingResponseDto>`
```ts
interface PayTransferSettingResponseDto {
  paytransferSettingId: string;   // uuid (สังเกต 't' ตัวเล็ก)
  isAutoTransfer: boolean;
  history: {
    createdByUser: string | null;
    employeeCode: string | null;
    isAutoTransfer: boolean;
    createdDate: string | null;
  }[] | null;
}
```

**UpdateSettingAutoTransfer** — Body:
```ts
{ paytransferSettingId: string }   // uuid, required
```
> ⚠️ **Body ไม่มีฟิลด์ `isAutoTransfer`** — API นี้น่าจะทำงานแบบ toggle สลับค่าเอง หรือ spec ตกหล่น **ต้องยืนยันกับ backend ก่อน implement** และหลังบันทึกควรเรียก `GetCurrentSetting` ซ้ำเพื่อ refresh ค่าจริง

**SearchClaimOrCase**
- Query: `searchDetail` *(string, **required**)*
- Response: `ServiceResponse<SearchClaimOrCaseResponseDto[]>`
```ts
interface SearchClaimOrCaseResponseDto {
  caseId: string;                    // uuid
  claimId: string | null;            // uuid
  claimCase: string | null;
  createdClaimDate: string | null;   // วันที่สร้างเคลม
  customerName: string | null;
  coverageType: string | null;       // ประเภทความคุ้มครอง
  caseAmount: number | null;         // จำนวนเงิน (case)
  isClaimNo: boolean;                // true = ที่ค้นหาเป็น ClaimNo, false = CaseNo
}
```
- ใช้ `isClaimNo` ตัดสินใจว่าจะแสดงคอลัมน์/flow แบบไหนต่อ

---

### 3.5 Notification — SMS & แบบประเมินความพึงพอใจ

| Method | Path | หน้าที่ |
|---|---|---|
| GET | `/api/Notification/GetSurveyId` | ขอ surveyId ปัจจุบัน |
| GET | `/api/Notification/GetTransactionById` | ดึง SMS transaction — query `payTransferTransactionId` (uuid) |
| POST | `/api/Notification/UpdateSMSTransactionSurvey` | ผูก surveyId เข้ากับ SMS transaction |
| POST | `/api/Notification/SaveSurveyFeedback` | บันทึกคำตอบแบบประเมิน |

```ts
// GetSurveyId → data
interface GetSurveyIdResponseDto { surveyId: number }

// GetTransactionById → data
interface GetSMSTransactionResponse {
  smStransactionId: string | null;   // uuid (สะกด 'smS' ตาม API)
  surveyId: number | null;
}

// UpdateSMSTransactionSurvey — body (ทั้งสองฟิลด์ required)
{ smStransactionId: string; surveyId: number }
// → data: { smStransactionId: string | null }

// SaveSurveyFeedback — body
{
  surveyId: number;        // required
  answersRequest: any;     // required — JToken (JSON อิสระ) ดูหมายเหตุ
}
// → data: { surveyId: number; message: string | null }
```

> ⚠️ `answersRequest` ถูกประกาศเป็น `JToken` ซึ่งใน swagger เป็น **recursive array schema** ที่ไม่มีความหมายจริง — ในทางปฏิบัติคือ JSON object/array อิสระ **ต้องขอตัวอย่าง payload จริงจาก backend** ฝั่ง TS ให้พิมพ์เป็น `unknown` แล้วค่อย narrow

**Flow ที่คาดว่าจะใช้:** โอนเงินสำเร็จ → ระบบส่ง SMS → ผู้รับเงินกดลิงก์ → หน้า survey เรียก `GetTransactionById` เพื่อรู้ว่า transaction ไหน → `GetSurveyId` → `UpdateSMSTransactionSurvey` → ผู้ใช้ตอบแบบสอบถาม → `SaveSurveyFeedback`

---

### 3.6 VersionControl

| Method | Path | หมายเหตุ |
|---|---|---|
| GET | `/api/VersionControl/GetApplicationVersionControl` | **ไม่ต้องใช้ token** — spec ไม่ระบุ schema ของ response |

---

## 4. Endpoint ที่ frontend ไม่ควรเรียก

- `POST /api/Transfer/v1/CreateTransferResult` — เป็น callback/webhook รับผลจากธนาคาร (`PayListResult`: `refCode`, `isSucceed`, `transRefNo`, `statusBank`, `transferDate`, `payResultStatusId`, ...) และ summary ระบุว่าเป็นการทดสอบ ไม่ใช่ flow ของ UI

---

## 5. Convention & ข้อควรระวัง

**การตั้งชื่อ** — มีจุดที่สะกดผิด/ไม่สม่ำเสมอใน API ห้าม "แก้ให้ถูก" ฝั่ง client:
| ที่ถูกต้องตาม API | หมายเหตุ |
|---|---|
| `itemCout` | ไม่ใช่ `itemCount` |
| `smStransactionId` | S ตัวใหญ่กลางคำ |
| `paytransferSettingId` | t ตัวเล็ก (ต่างจาก `PayTransferSetting...`) |
| `AdjustmentReasonId` | query param เดียวที่ขึ้นต้นตัวใหญ่ |
| `HospitalSettingMonitorReponseDto` | "Reponse" |

**วันที่/เวลา** — ทุกฟิลด์เป็น ISO 8601 string. ตรวจสอบ timezone กับ backend ว่าเป็น UTC หรือ +07:00 ก่อน format แสดงผล (แนะนำ `dayjs` + plugin `utc`/`timezone`)

**จำนวนเงิน** — เป็น `double` ควรจัดรูปแบบด้วย `Intl.NumberFormat('th-TH', { minimumFractionDigits: 2 })` และอย่าทำ arithmetic ทศนิยมบน UI โดยไม่ระวัง floating point

**Paging** — envelope มีฟิลด์ paging ครบ แต่**ยังไม่มี endpoint ไหนรับ query param สำหรับ page/size** → ต้อง paginate ฝั่ง client ไปก่อน และเตรียมรองรับเมื่อ backend เปิดใช้

**CORS / DevTunnel** — base URL ปัจจุบันเป็น VS Code dev tunnel ซึ่งเปลี่ยนได้ทุกครั้งที่ restart → เก็บไว้ใน `.env` (`VITE_API_BASE_URL`) ห้าม hardcode

---

## 6. ตัวอย่างโค้ด React

Stack: **axios + @tanstack/react-query**. โครงสร้างไฟล์แนะนำ:

```
src/
  api/
    http.ts            # axios instance + interceptor
    types.ts           # ServiceResponse<T> + DTOs
    hospitalSetting.ts
    masters.ts
    transfer.ts
    setting.ts
    notification.ts
  hooks/
    useHospitalSetting.ts
    useMasters.ts
    useTransfer.ts
    ...
```

### 6.1 `api/http.ts` — axios instance + auth interceptor

```ts
import axios, { AxiosError } from "axios";

export const http = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL, // เช่น https://s9ps9j5t-5001.asse.devtunnels.ms
  headers: { "Content-Type": "application/json" },
});

// แนบ token ทุก request
http.interceptors.request.use((config) => {
  const token = getAccessToken(); // ดึงจาก store/oidc-client ของคุณ
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// ดัก 401 → refresh แล้ว retry 1 ครั้ง
http.interceptors.response.use(
  (res) => res,
  async (error: AxiosError) => {
    const original = error.config as any;
    if (error.response?.status === 401 && !original._retry) {
      original._retry = true;
      const newToken = await refreshAccessToken(); // implement ตาม oidc flow
      if (newToken) {
        original.headers.Authorization = `Bearer ${newToken}`;
        return http(original);
      }
    }
    if (error.response?.status === 403) {
      // redirect ไปหน้า no-permission ตามที่ระบบกำหนด
    }
    return Promise.reject(error);
  }
);
```

### 6.2 `api/types.ts` — envelope + helper กลาง

```ts
export interface ServiceResponse<T> {
  data: T | null;
  isSuccess: boolean;
  message: string | null;
  code: number | null;
  exceptionMessage: unknown | null;
  serverDateTime: string;
  totalAmountRecords: number | null;
  totalAmountPages: number | null;
  currentPage: number | null;
  recordsPerPage: number | null;
  pageIndex: number | null;
}

export class ApiError extends Error {
  code: number | null;
  constructor(message: string | null, code: number | null) {
    super(message ?? "Unknown API error");
    this.code = code;
  }
}

// unwrap envelope กลาง ใช้ทุกที่ที่เรียก API
export function unwrap<T>(res: { data: ServiceResponse<T> }): T {
  const body = res.data;
  if (!body.isSuccess) throw new ApiError(body.message, body.code);
  if (body.data === null) throw new ApiError("No data returned", body.code);
  return body.data;
}
```

### 6.3 `api/masters.ts` — ตัวอย่าง GET แบบ list + object

```ts
import { http } from "./http";
import { ServiceResponse, unwrap } from "./types";

export interface MasterItem {
  id: number;
  name: string | null;
}

export const mastersApi = {
  getAdjustmentReasons: () =>
    http
      .get<ServiceResponse<MasterItem[]>>("/api/Masters/GetAdjustmentReasons")
      .then(unwrap),

  getTransferStatuses: () =>
    http
      .get<ServiceResponse<MasterItem[]>>("/api/Masters/GetTransferStatuses")
      .then(unwrap),

  getPaymentStatuses: () =>
    http
      .get<ServiceResponse<MasterItem[]>>("/api/Masters/GetPaymentStatuses")
      .then(unwrap),

  getBankAccountRelationTypes: () =>
    http
      .get<ServiceResponse<MasterItem[]>>(
        "/api/Masters/GetBankAccountRelationTypes"
      )
      .then(unwrap),
};
```

### 6.4 `hooks/useMasters.ts` — React Query, cache แบบ static data

```ts
import { useQuery } from "@tanstack/react-query";
import { mastersApi } from "../api/masters";

export function useAdjustmentReasons() {
  return useQuery({
    queryKey: ["masters", "adjustmentReasons"],
    queryFn: mastersApi.getAdjustmentReasons,
    staleTime: Infinity, // master data ไม่เปลี่ยนบ่อย ไม่ต้องยิงซ้ำ
    gcTime: Infinity,
  });
}

export function useTransferStatuses() {
  return useQuery({
    queryKey: ["masters", "transferStatuses"],
    queryFn: mastersApi.getTransferStatuses,
    staleTime: Infinity,
  });
}
```

### 6.5 `api/hospitalSetting.ts` + hook — search แบบมี debounce

```ts
// api/hospitalSetting.ts
import { http } from "./http";
import { ServiceResponse, unwrap } from "./types";

export interface SearchHospitalResponseDto {
  orgId: number;
  orgName: string | null;
}

export interface HospitalSettingMonitorReponseDto {
  hospitalPaymentSettingId: string;
  hospitalName: string | null;
  isAutoPay: boolean | null;
  delayDays: number;
  holdStatusId: number | null;
  holdStatusName: string | null;
  updatedDate: string | null;
}

export interface AddHospitalSettingRequestDto {
  hospitalId: number;
  hospitalName: string;
  autoPayDelayDays: number;
  holdStatusId: number;
  isAutoPay: boolean;
}

export const hospitalSettingApi = {
  search: (orgName: string) =>
    http
      .get<ServiceResponse<SearchHospitalResponseDto[]>>(
        "/api/HospitalPaymentSetting/Finance/SearchHospital",
        { params: { orgName } }
      )
      .then(unwrap),

  monitor: (searchDetail?: string) =>
    http
      .get<ServiceResponse<HospitalSettingMonitorReponseDto[]>>(
        "/api/HospitalPaymentSetting/Finance/HospitalSettingMonitor",
        { params: { searchDetail } }
      )
      .then(unwrap),

  add: (payload: AddHospitalSettingRequestDto) =>
    http
      .post<ServiceResponse<{ isSuccess: boolean; message: string | null }>>(
        "/api/HospitalPaymentSetting/Finance/AddHospitalSetting",
        payload
      )
      .then(unwrap),
};
```

```tsx
// hooks/useHospitalSearch.ts — autocomplete พร้อม debounce
import { useQuery } from "@tanstack/react-query";
import { useMemo, useState } from "react";
import { hospitalSettingApi } from "../api/hospitalSetting";

function useDebounce<T>(value: T, delay = 300): T {
  const [debounced, setDebounced] = useState(value);
  useMemo(() => {
    const t = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(t);
  }, [value, delay]);
  return debounced;
}

export function useHospitalSearch(orgName: string) {
  const debounced = useDebounce(orgName, 300);
  return useQuery({
    queryKey: ["hospitalSearch", debounced],
    queryFn: () => hospitalSettingApi.search(debounced),
    enabled: debounced.length > 0, // orgName required — อย่ายิงตอนว่าง
  });
}
```

```tsx
// component ตัวอย่าง
function HospitalAutocomplete({ onSelect }: { onSelect: (orgId: number) => void }) {
  const [text, setText] = useState("");
  const { data, isLoading } = useHospitalSearch(text);

  return (
    <div>
      <input value={text} onChange={(e) => setText(e.target.value)} placeholder="ค้นหาสถานพยาบาล" />
      {isLoading && <span>กำลังค้นหา...</span>}
      <ul>
        {data?.map((h) => (
          <li key={h.orgId} onClick={() => onSelect(h.orgId)}>
            {h.orgName}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### 6.6 `api/transfer.ts` + hook — POST ที่โอนเงินจริง (mutation + confirm + กัน double-submit)

```ts
// api/transfer.ts
import { http } from "./http";
import { ServiceResponse, unwrap } from "./types";

export interface CreatePaymentRequestDto {
  casePayableId: string;
  grossPaidAmount: number;
  withHoldingTaxAmount: number;
  netPaidAmount: number;
  receivingBankId: number;
  receivingBankAccountNo: string;
  receivingBankName: string;
  receivingAccountName: string;
  phoneNumber: string;
  claimCase: string;
  claimNo: string;
  payeeTypeId?: number;
  paymentTypeId?: number;
  userId?: number;
}

export interface CreatePaymentResponseDto {
  itemCout: number;
  totalNetPaidAmount: number | null;
  isSuccess: boolean;
  message: string | null;
  payListHeaderId: string | null;
  refTransactionId: string | null;
  paymentCodeResponse: { paymentCode: string | null }[] | null;
}

export interface PaymentTransactionResponseDto {
  paymentDate: string | null;
  printDate: string | null;
  admissionDate: string | null;
  countItem: number;
  paymentId: string;
  totalNetPaidAmount: number | null;
  customerName: string | null;
  employee: string | null;
  accountNo: string | null;
  accountName: string | null;
  bankName: string | null;
  paymentCode: string | null;
  paymentItemDetail:
    | { claimNo: string | null; customerName: string | null; transactionAmount: number | null; bankName: string | null; approvedAmount: number | null }[]
    | null;
}

export const transferApi = {
  createPayment: (items: CreatePaymentRequestDto[]) =>
    http
      .post<ServiceResponse<CreatePaymentResponseDto>>(
        "/api/Transfer/v1/CreatePayment",
        items
      )
      .then(unwrap),

  getPaymentDetails: (referenceCode: string) =>
    http
      .get<ServiceResponse<PaymentTransactionResponseDto>>(
        "/api/Transfer/v1/PaymentDetails",
        { params: { referenceCode } }
      )
      .then(unwrap),
};
```

```tsx
// hooks/useCreatePayment.ts
import { useMutation } from "@tanstack/react-query";
import { transferApi, CreatePaymentRequestDto } from "../api/transfer";

export function useCreatePayment() {
  return useMutation({
    mutationFn: (items: CreatePaymentRequestDto[]) =>
      transferApi.createPayment(items),
  });
}
```

```tsx
// component ตัวอย่าง — confirm ก่อนโอน + disable ปุ่มกัน double-submit
function ConfirmPaymentButton({ payload }: { payload: CreatePaymentRequestDto[] }) {
  const { mutate, isPending } = useCreatePayment();
  const [confirmOpen, setConfirmOpen] = useState(false);

  const handleConfirm = () => {
    setConfirmOpen(false);
    mutate(payload, {
      onSuccess: (data) => {
        // data.refTransactionId → ใช้เรียก getPaymentDetails ต่อ
        toast.success(`โอนเงินสำเร็จ ${data.itemCout} รายการ`);
      },
      onError: (err) => {
        toast.error(err instanceof ApiError ? err.message : "เกิดข้อผิดพลาด");
      },
    });
  };

  return (
    <>
      <button disabled={isPending} onClick={() => setConfirmOpen(true)}>
        {isPending ? "กำลังโอนเงิน..." : "ยืนยันการโอนเงิน"}
      </button>
      {confirmOpen && (
        <ConfirmDialog
          message={`ยืนยันโอนเงิน ${payload.length} รายการ?`}
          onConfirm={handleConfirm}
          onCancel={() => setConfirmOpen(false)}
        />
      )}
    </>
  );
}
```

> จุดสำคัญ: `disabled={isPending}` + ปุ่มถูก disable ทันทีที่กด confirm คือแนวป้องกัน double-submit หลักในเมื่อ API ไม่มี idempotency key ให้ฝั่ง client เอง — **ห้าม** ให้ user กดซ้ำได้ระหว่างรอ response

### 6.7 Global error handling (ตัวอย่าง React Query defaults)

```tsx
// main.tsx
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { ApiError } from "./api/types";

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: (failureCount, error) => {
        // ไม่ retry ถ้าเป็น business error (isSuccess: false) หรือ 401/403
        if (error instanceof ApiError) return false;
        return failureCount < 2;
      },
    },
    mutations: {
      onError: (error) => {
        console.error(error); // ส่งเข้า logging service จริง เช่น Sentry
      },
    },
  },
});
```

---

## 7. คำถามที่ต้องเคลียร์กับ Backend

1. `UpdateSettingAutoTransfer` รับแค่ `paytransferSettingId` — จะส่งค่า `isAutoTransfer` ใหม่ยังไง?
2. `holdStatusId` ดึงรายการมาจาก master ตัวไหน?
3. `answersRequest` ของ `SaveSurveyFeedback` มีโครงสร้างจริงอย่างไร?
4. `payeeTypeId` / `paymentTypeId` ใน `CreatePayment` มีค่าอะไรบ้าง และ optional จริงหรือไม่?
5. `CreatePayment` มี partial success ไหม (บางรายการในอาร์เรย์สำเร็จ บางรายการล้มเหลว) และ error response หน้าตาเป็นอย่างไร?
6. Paging จะเปิดใช้เมื่อไหร่ และใช้ query param ชื่ออะไร?
7. `GetApplicationVersionControl` คืน schema แบบไหน?
