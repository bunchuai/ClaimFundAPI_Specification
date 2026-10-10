# ClaimFundAPI — API Specification

> **เวอร์ชัน:** v1.0.0  |  **มาตรฐาน:** OpenAPI 3.0.1  |  **อัปเดตล่าสุด:** 10 ตุลาคม 2569 (2026-10-10)

> **ที่มาของ Spec:** ไฟล์ `ClaimfunAPISpecification.json` (swagger.json จาก `https://claimfundapi.uatsiamsmile.com`, สภาพแวดล้อม UAT)

<a id="toc"></a>
## สารบัญ

1. [ภาพรวมระบบ](#overview)
2. [การยืนยันตัวตนและสิทธิ์การเข้าถึง](#authentication)
3. [รูปแบบการเรียกใช้และ Response มาตรฐาน](#conventions)
4. [สรุปรายการ Endpoint ทั้งหมด](#endpoint-summary)
5. [รายละเอียด Endpoint](#endpoints)
    - 5.1 [การตั้งค่าการจ่ายเงินโรงพยาบาล (`HospitalPaymentSetting`)](#group-hospitalpaymentsetting)
        - 5.1.1 [`GET` SearchHospital — ค้นหา ชื่อสถานพยาบาล](#ep-get-api-hospitalpaymentsetting-finance-searchhospital)
        - 5.1.2 [`GET` HospitalSettingMonitor — รายการตั้งค่าการจ่ายเงิน](#ep-get-api-hospitalpaymentsetting-finance-hospitalsettingmonitor)
        - 5.1.3 [`POST` AddHospitalSetting — เพิ่มสถานพยาบาลที่ต้องการตั้งค่า](#ep-post-api-hospitalpaymentsetting-finance-addhospitalsetting)
        - 5.1.4 [`POST` UpdateHospitalSetting — แก้ไข สถานพยาบาลที่ต้องการตั้งค่า](#ep-post-api-hospitalpaymentsetting-finance-updatehospitalsetting)
        - 5.1.5 [`GET` GetHistories — ประวัติการตั้งค่า](#ep-get-api-hospitalpaymentsetting-finance-gethistories)
    - 5.2 [การโอนเงินโรงพยาบาล (`HospitalTransfer`)](#group-hospitaltransfer)
        - 5.2.1 [`POST` HospitalTransferMonitor — รายการรอกรุ๊ปโอนเงินโรงพยาบาล](#ep-post-api-hospitaltransfer-v1-hospitaltransfermonitor)
        - 5.2.2 [`POST` HospitalTransferStatusMonitor — สถานะการโอนเงินโรงพยาบาล](#ep-post-api-hospitaltransfer-v1-hospitaltransferstatusmonitor)
        - 5.2.3 [`GET` HospitalTransferDetail — รายละเอียดรายการโอนเงินโรงพยาบาล](#ep-get-api-hospitaltransfer-v1-hospitaltransferdetail)
        - 5.2.4 [`POST` TransferHospital — ส่งรายการจ่ายเงินโรงพยาบาลไปยังระบบโอนเงิน](#ep-post-api-hospitaltransfer-v1-transferhospital)
        - 5.2.5 [`POST` CreatePaymentHospital — สร้างรายการ กรุ๊ปโอนเงินโรงพยาบาล ให้เป็นสถานะรอโอน](#ep-post-api-hospitaltransfer-v1-createpaymenthospital)
        - 5.2.6 [`POST` TransferHospitalGroup — โอนเงินโรงพยาบาลแบบกรุ๊ป](#ep-post-api-hospitaltransfer-v1-transferhospitalgroup)
        - 5.2.7 [`POST` SendEmailToHospital — ส่ง Email ผลการโอนเงินให้ รพ.](#ep-post-api-hospitaltransfer-v1-sendemailtohospital)
    - 5.3 [ข้อมูลหลัก (Master Data) (`Masters`)](#group-masters)
        - 5.3.1 [`GET` GetAdjustmentReasons — ข้อมูลสาเหตุการโอนเพิ่ม](#ep-get-api-masters-getadjustmentreasons)
        - 5.3.2 [`GET` GetAdjustmentReasonById — ข้อมูลสาเหตุการโอนเพิ่ม By Id](#ep-get-api-masters-getadjustmentreasonbyid)
        - 5.3.3 [`GET` GetTransferStatuses — ข้อมูลสถานะการโอนเงิน (Transfer Status)](#ep-get-api-masters-gettransferstatuses)
        - 5.3.4 [`GET` GetPaymentStatuses — ข้อมูลสถานะการจ่ายเงิน (Payment Status)](#ep-get-api-masters-getpaymentstatuses)
        - 5.3.5 [`GET` GetHospitalTransferGroupStatuses — ข้อมูลสถานะรายการของกรุ๊ปโอนเงินโรงพยาบาล (สำหรับ filter หน้าจอค้นหารายการโอนเงิน)](#ep-get-api-masters-gethospitaltransfergroupstatuses)
        - 5.3.6 [`GET` GetBankAccountRelationTypes — ข้อมูลประเภทความสัมพันธ์ของบัญชี กับผู้รับสินไหม](#ep-get-api-masters-getbankaccountrelationtypes)
        - 5.3.7 [`GET` GetBankAccountRelationTypeById — ข้อมูลประเภทความสัมพันธ์ของบัญชี กับผู้รับสินไหม By Id](#ep-get-api-masters-getbankaccountrelationtypebyid)
    - 5.4 [การแจ้งเตือนและแบบประเมิน (`Notification`)](#group-notification)
        - 5.4.1 [`GET` GetTransactionById — Get Transaction ของรายการส่ง SMS](#ep-get-api-notification-gettransactionbyid)
        - 5.4.2 [`POST` UpdateSMSTransactionSurvey — Update SurveyId ใน SMSTransaction](#ep-post-api-notification-updatesmstransactionsurvey)
        - 5.4.3 [`POST` SaveSurveyFeedback — ผลตอบแบบประเมิน](#ep-post-api-notification-savesurveyfeedback)
        - 5.4.4 [`GET` GetSurveyId — Get surveyId](#ep-get-api-notification-getsurveyid)
    - 5.5 [ยอดเงินคงเหลือ (`RemainingBalance`)](#group-remainingbalance)
        - 5.5.1 [`POST` InsertRemainingBalance — Insert ยอดเงินคงเหลือของบัญชี](#ep-post-api-remainingbalance-insertremainingbalance)
        - 5.5.2 [`GET` InsertRemainingBalance — Get ข้อมูลยอดเงินคงเหลือในบัญชี](#ep-get-api-remainingbalance-insertremainingbalance)
    - 5.6 [การตั้งค่าการโอนเงิน (`Setting`)](#group-setting)
        - 5.6.1 [`GET` GetCurrentSetting — ตั้งค่าการโอนเงิน](#ep-get-api-setting-getcurrentsetting)
        - 5.6.2 [`POST` UpdateSettingAutoTransfer — บันทึกการตั้งค่าการโอนเงิน](#ep-post-api-setting-updatesettingautotransfer)
        - 5.6.3 [`GET` SearchClaimOrCase — ค้นหารายการจาก เลขที่ ClaimNo / CaseNo](#ep-get-api-setting-searchclaimorcase)
    - 5.7 [การโอนเงิน (`Transfer`)](#group-transfer)
        - 5.7.1 [`POST` CreateTransferResult — Step 3 CreatedTransfer ทดสอบผลการสร้างรายการโอนเงิน](#ep-post-api-transfer-v1-createtransferresult)
        - 5.7.2 [`POST` CreatePayment — สร้างรายการโอนเงิน และโอนเงิน](#ep-post-api-transfer-v1-createpayment)
        - 5.7.3 [`GET` PaymentDetails — รายละเอียดรายการชำระเงิน (ตาม referenceCode)](#ep-get-api-transfer-v1-paymentdetails)
    - 5.8 [เวอร์ชันแอปพลิเคชัน (`VersionControl`)](#group-versioncontrol)
        - 5.8.1 [`GET` GetApplicationVersionControl — Version ของ Application](#ep-get-version-getapplicationversioncontrol)
6. [Data Models (Schemas)](#schemas)
7. [ข้อสังเกตและข้อควรระวังจากการวิเคราะห์ Spec](#notes)
8. [บันทึกการเปลี่ยนแปลง (Changelog)](#changelog)

---

<a id="overview"></a>
## 1. ภาพรวมระบบ

ClaimFundAPI เป็นบริการกลางสำหรับการโอนเงินเพื่อจ่ายค่าสินไหมทดแทนของการเคลมประกันภัย โดยรองรับการโอนเงินอย่างปลอดภัย การตรวจสอบความถูกต้องของบัญชีผู้รับเงิน การติดตามรายการธุรกรรม และการจัดการสถานะการชำระเงิน ผ่าน REST API ที่เป็นมาตรฐาน

- **จำนวน Endpoint:** 32 รายการ (8 กลุ่ม)
- **จำนวน Schema:** 72 รายการ
- **รูปแบบข้อมูล:** JSON ผ่าน HTTPS (REST)
- **Base URL (UAT):** `https://claimfundapi.uatsiamsmile.com` — อนุมานจากที่อยู่ของ swagger.json เนื่องจาก spec ไม่ได้ระบุ `servers`

| กลุ่ม | คำอธิบาย | จำนวน Endpoint |
|---|---|:---:|
| [การตั้งค่าการจ่ายเงินโรงพยาบาล](#group-hospitalpaymentsetting) | จัดการสถานพยาบาลที่ต้องตั้งค่าการจ่ายเงิน ค้นหา เพิ่ม แก้ไข และดูประวัติการตั้งค่า | 5 |
| [การโอนเงินโรงพยาบาล](#group-hospitaltransfer) | จัดกลุ่มรายการจ่ายเงิน ส่งโอน ติดตามสถานะ และส่งอีเมลแจ้งผลการโอนให้โรงพยาบาล | 7 |
| [ข้อมูลหลัก (Master Data)](#group-masters) | ข้อมูลอ้างอิงสำหรับ dropdown/filter เช่น สถานะการโอน สถานะการจ่าย สาเหตุการโอนเพิ่ม และประเภทความสัมพันธ์ของบัญชี | 7 |
| [การแจ้งเตือนและแบบประเมิน](#group-notification) | ข้อมูลรายการ SMS และการจัดการแบบประเมินความพึงพอใจ (Survey) | 4 |
| [ยอดเงินคงเหลือ](#group-remainingbalance) | บันทึกและเรียกดูยอดเงินคงเหลือของบัญชีธนาคาร | 2 |
| [การตั้งค่าการโอนเงิน](#group-setting) | ตั้งค่าการโอนเงินอัตโนมัติ และค้นหารายการจากเลขที่ ClaimNo / CaseNo | 3 |
| [การโอนเงิน](#group-transfer) | สร้างรายการโอนเงินค่าสินไหมทดแทน ตรวจสอบรายละเอียด และรับผลการโอน | 3 |
| [เวอร์ชันแอปพลิเคชัน](#group-versioncontrol) | ตรวจสอบเวอร์ชันของแอปพลิเคชัน | 1 |

[⬆ กลับสู่สารบัญ](#toc)

<a id="authentication"></a>
## 2. การยืนยันตัวตนและสิทธิ์การเข้าถึง

### 2.1 OAuth 2.0 (Authorization Code Flow)

Endpoint ส่วนใหญ่ต้องแนบ Access Token ใน Header:

```http
Authorization: Bearer <access_token>
```

| รายการ | ค่า |
|---|---|
| Flow | Authorization Code |
| Authorization URL | `https://authlogin.uatsiamsmile.com/connect/authorize` |
| Token URL | `https://authlogin.uatsiamsmile.com/connect/token` |

**Scopes**

| Scope | คำอธิบาย |
|---|---|
| `ClaimFundAPI` | ClaimFundAPI API |
| `openid` | OpenId |
| `profile` | User's profile |
| `roles` | User's role |

> Endpoint ที่ต้องใช้ Token ต้องได้รับ scope `ClaimFundAPI`

> ⚠️ URL ข้างต้นเป็นสภาพแวดล้อม **UAT** (`uatsiamsmile.com`) ตามที่ระบุใน spec — ตรวจสอบค่าสำหรับ Production กับผู้ดูแลระบบ

### 2.2 Endpoint ที่ไม่ใช้ OAuth 2.0 / ใช้ API Key

| Endpoint | วิธีการ |
|---|---|
| `POST /api/RemainingBalance/InsertRemainingBalance` | Header `apiKey` |
| `GET /api/RemainingBalance/InsertRemainingBalance` | ไม่ระบุการยืนยันตัวตนใน spec |
| `GET /Version/GetApplicationVersionControl` | ไม่ระบุการยืนยันตัวตนใน spec |

**Endpoint ที่ต้องใช้ทั้ง OAuth 2.0 และ Header `apiKey`**

| Endpoint |
|---|
| `GET /api/Notification/GetTransactionById` |
| `POST /api/Notification/UpdateSMSTransactionSurvey` |
| `POST /api/Notification/SaveSurveyFeedback` |
| `GET /api/Notification/GetSurveyId` |

### 2.3 HTTP Status Codes

| Code | ความหมาย |
|---|---|
| `200` | Success |
| `401` | Unauthorized — ไม่มี/Token ไม่ถูกต้อง หรือหมดอายุ |
| `403` | Forbidden — ไม่มีสิทธิ์เข้าถึง (scope/role ไม่เพียงพอ) |

[⬆ กลับสู่สารบัญ](#toc)

<a id="conventions"></a>
## 3. รูปแบบการเรียกใช้และ Response มาตรฐาน

### 3.1 Content Type

- **Request Body:** แนะนำให้ใช้ `application/json` (spec ยังรองรับ `application/json-patch+json`, `text/json`, `application/*+json`)
- **Response:** `application/json` (spec ระบุ `text/plain` และ `text/json` ด้วย)

### 3.2 โครงสร้าง Response มาตรฐาน (`ServiceResponse`)

Endpoint ส่วนใหญ่ตอบกลับด้วย Envelope เดียวกัน โดยข้อมูลจริงอยู่ในฟิลด์ `data` (เป็น object หรือ array ตาม Endpoint)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `data` | any | — | — | ข้อมูลผลลัพธ์ (ชนิดขึ้นกับแต่ละ Endpoint) |
| `isSuccess` | boolean | — | — | ผลการทำงาน `true` = สำเร็จ |
| `message` | string | — | ได้ | ข้อความอธิบายผลลัพธ์/ข้อผิดพลาด |
| `code` | integer (int32) | — | ได้ | รหัสผลลัพธ์ของระบบ |
| `exceptionMessage` | any | — | ได้ | รายละเอียด Exception (กรณีเกิดข้อผิดพลาด) |
| `serverDateTime` | string (date-time) | — | — | วันเวลาของเซิร์ฟเวอร์ ณ เวลาที่ตอบกลับ |
| `totalAmountRecords` | number (double) | — | ได้ | จำนวนรายการทั้งหมด (ใช้กับ Endpoint ที่แบ่งหน้า) |
| `totalAmountPages` | number (double) | — | ได้ | จำนวนหน้าทั้งหมด |
| `currentPage` | number (double) | — | ได้ | หน้าปัจจุบัน |
| `recordsPerPage` | number (double) | — | ได้ | จำนวนรายการต่อหน้า |
| `pageIndex` | integer (int32) | — | ได้ | ดัชนีหน้า |

> คำอธิบายฟิลด์ใน Envelope เขียนเพิ่มโดยผู้จัดทำเอกสาร (spec ต้นฉบับไม่มี description) โปรดยืนยันกับทีมพัฒนา

**ตัวอย่าง**

```json
{
  "data": {
    "...": "..."
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z",
  "totalAmountRecords": 100.0,
  "totalAmountPages": 10.0,
  "currentPage": 1.0,
  "recordsPerPage": 10.0,
  "pageIndex": 1
}
```

### 3.3 การแบ่งหน้า (Pagination)

Endpoint `HospitalTransferMonitor` และ `HospitalTransferStatusMonitor` รับ Query Parameter `Page` และ `RecordsPerPage` และคืนข้อมูลแบ่งหน้าใน Envelope (`totalAmountRecords`, `totalAmountPages`, `currentPage`, `recordsPerPage`)

[⬆ กลับสู่สารบัญ](#toc)

<a id="endpoint-summary"></a>
## 4. สรุปรายการ Endpoint ทั้งหมด

| # | Method | Path | คำอธิบาย | Auth |
|:---:|:---:|---|---|:---:|
| 5.1.1 | `GET` | [`/api/HospitalPaymentSetting/Finance/SearchHospital`](#ep-get-api-hospitalpaymentsetting-finance-searchhospital) | ค้นหา ชื่อสถานพยาบาล | OAuth2 |
| 5.1.2 | `GET` | [`/api/HospitalPaymentSetting/Finance/HospitalSettingMonitor`](#ep-get-api-hospitalpaymentsetting-finance-hospitalsettingmonitor) | รายการตั้งค่าการจ่ายเงิน | OAuth2 |
| 5.1.3 | `POST` | [`/api/HospitalPaymentSetting/Finance/AddHospitalSetting`](#ep-post-api-hospitalpaymentsetting-finance-addhospitalsetting) | เพิ่มสถานพยาบาลที่ต้องการตั้งค่า | OAuth2 |
| 5.1.4 | `POST` | [`/api/HospitalPaymentSetting/Finance/UpdateHospitalSetting`](#ep-post-api-hospitalpaymentsetting-finance-updatehospitalsetting) | แก้ไข สถานพยาบาลที่ต้องการตั้งค่า | OAuth2 |
| 5.1.5 | `GET` | [`/api/HospitalPaymentSetting/Finance/GetHistories`](#ep-get-api-hospitalpaymentsetting-finance-gethistories) | ประวัติการตั้งค่า | OAuth2 |
| 5.2.1 | `POST` | [`/api/HospitalTransfer/v1/HospitalTransferMonitor`](#ep-post-api-hospitaltransfer-v1-hospitaltransfermonitor) | รายการรอกรุ๊ปโอนเงินโรงพยาบาล | OAuth2 |
| 5.2.2 | `POST` | [`/api/HospitalTransfer/v1/HospitalTransferStatusMonitor`](#ep-post-api-hospitaltransfer-v1-hospitaltransferstatusmonitor) | สถานะการโอนเงินโรงพยาบาล | OAuth2 |
| 5.2.3 | `GET` | [`/api/HospitalTransfer/v1/HospitalTransferDetail`](#ep-get-api-hospitaltransfer-v1-hospitaltransferdetail) | รายละเอียดรายการโอนเงินโรงพยาบาล | OAuth2 |
| 5.2.4 | `POST` | [`/api/HospitalTransfer/v1/TransferHospital`](#ep-post-api-hospitaltransfer-v1-transferhospital) | ส่งรายการจ่ายเงินโรงพยาบาลไปยังระบบโอนเงิน | OAuth2 |
| 5.2.5 | `POST` | [`/api/HospitalTransfer/v1/CreatePaymentHospital`](#ep-post-api-hospitaltransfer-v1-createpaymenthospital) | สร้างรายการ กรุ๊ปโอนเงินโรงพยาบาล ให้เป็นสถานะรอโอน | OAuth2 |
| 5.2.6 | `POST` | [`/api/HospitalTransfer/v1/TransferHospitalGroup`](#ep-post-api-hospitaltransfer-v1-transferhospitalgroup) | โอนเงินโรงพยาบาลแบบกรุ๊ป | OAuth2 |
| 5.2.7 | `POST` | [`/api/HospitalTransfer/v1/SendEmailToHospital`](#ep-post-api-hospitaltransfer-v1-sendemailtohospital) | ส่ง Email ผลการโอนเงินให้ รพ. | OAuth2 |
| 5.3.1 | `GET` | [`/api/Masters/GetAdjustmentReasons`](#ep-get-api-masters-getadjustmentreasons) | ข้อมูลสาเหตุการโอนเพิ่ม | OAuth2 |
| 5.3.2 | `GET` | [`/api/Masters/GetAdjustmentReasonById`](#ep-get-api-masters-getadjustmentreasonbyid) | ข้อมูลสาเหตุการโอนเพิ่ม By Id | OAuth2 |
| 5.3.3 | `GET` | [`/api/Masters/GetTransferStatuses`](#ep-get-api-masters-gettransferstatuses) | ข้อมูลสถานะการโอนเงิน (Transfer Status) | OAuth2 |
| 5.3.4 | `GET` | [`/api/Masters/GetPaymentStatuses`](#ep-get-api-masters-getpaymentstatuses) | ข้อมูลสถานะการจ่ายเงิน (Payment Status) | OAuth2 |
| 5.3.5 | `GET` | [`/api/Masters/GetHospitalTransferGroupStatuses`](#ep-get-api-masters-gethospitaltransfergroupstatuses) | ข้อมูลสถานะรายการของกรุ๊ปโอนเงินโรงพยาบาล (สำหรับ filter หน้าจอค้นหารายการโอนเงิน) | OAuth2 |
| 5.3.6 | `GET` | [`/api/Masters/GetBankAccountRelationTypes`](#ep-get-api-masters-getbankaccountrelationtypes) | ข้อมูลประเภทความสัมพันธ์ของบัญชี กับผู้รับสินไหม | OAuth2 |
| 5.3.7 | `GET` | [`/api/Masters/GetBankAccountRelationTypeById`](#ep-get-api-masters-getbankaccountrelationtypebyid) | ข้อมูลประเภทความสัมพันธ์ของบัญชี กับผู้รับสินไหม By Id | OAuth2 |
| 5.4.1 | `GET` | [`/api/Notification/GetTransactionById`](#ep-get-api-notification-gettransactionbyid) | Get Transaction ของรายการส่ง SMS | OAuth2 + API Key |
| 5.4.2 | `POST` | [`/api/Notification/UpdateSMSTransactionSurvey`](#ep-post-api-notification-updatesmstransactionsurvey) | Update SurveyId ใน SMSTransaction | OAuth2 + API Key |
| 5.4.3 | `POST` | [`/api/Notification/SaveSurveyFeedback`](#ep-post-api-notification-savesurveyfeedback) | ผลตอบแบบประเมิน | OAuth2 + API Key |
| 5.4.4 | `GET` | [`/api/Notification/GetSurveyId`](#ep-get-api-notification-getsurveyid) | Get surveyId | OAuth2 + API Key |
| 5.5.1 | `POST` | [`/api/RemainingBalance/InsertRemainingBalance`](#ep-post-api-remainingbalance-insertremainingbalance) | Insert ยอดเงินคงเหลือของบัญชี | API Key |
| 5.5.2 | `GET` | [`/api/RemainingBalance/InsertRemainingBalance`](#ep-get-api-remainingbalance-insertremainingbalance) | Get ข้อมูลยอดเงินคงเหลือในบัญชี | ไม่ระบุ |
| 5.6.1 | `GET` | [`/api/Setting/GetCurrentSetting`](#ep-get-api-setting-getcurrentsetting) | ตั้งค่าการโอนเงิน | OAuth2 |
| 5.6.2 | `POST` | [`/api/Setting/UpdateSettingAutoTransfer`](#ep-post-api-setting-updatesettingautotransfer) | บันทึกการตั้งค่าการโอนเงิน | OAuth2 |
| 5.6.3 | `GET` | [`/api/Setting/SearchClaimOrCase`](#ep-get-api-setting-searchclaimorcase) | ค้นหารายการจาก เลขที่ ClaimNo / CaseNo | OAuth2 |
| 5.7.1 | `POST` | [`/api/Transfer/v1/CreateTransferResult`](#ep-post-api-transfer-v1-createtransferresult) | Step 3 CreatedTransfer ทดสอบผลการสร้างรายการโอนเงิน | OAuth2 |
| 5.7.2 | `POST` | [`/api/Transfer/v1/CreatePayment`](#ep-post-api-transfer-v1-createpayment) | สร้างรายการโอนเงิน และโอนเงิน | OAuth2 |
| 5.7.3 | `GET` | [`/api/Transfer/v1/PaymentDetails`](#ep-get-api-transfer-v1-paymentdetails) | รายละเอียดรายการชำระเงิน (ตาม referenceCode) | OAuth2 |
| 5.8.1 | `GET` | [`/Version/GetApplicationVersionControl`](#ep-get-version-getapplicationversioncontrol) | Version ของ Application | ไม่ระบุ |

[⬆ กลับสู่สารบัญ](#toc)

<a id="endpoints"></a>
## 5. รายละเอียด Endpoint

<a id="group-hospitalpaymentsetting"></a>
### 5.1 การตั้งค่าการจ่ายเงินโรงพยาบาล (`HospitalPaymentSetting`)

จัดการสถานพยาบาลที่ต้องตั้งค่าการจ่ายเงิน ค้นหา เพิ่ม แก้ไข และดูประวัติการตั้งค่า

<a id="ep-get-api-hospitalpaymentsetting-finance-searchhospital"></a>
#### 5.1.1 ค้นหา ชื่อสถานพยาบาล

```http
GET /api/HospitalPaymentSetting/Finance/SearchHospital
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `orgName` | query | string | ✅ |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [SearchHospitalResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [SearchHospitalResponseDto](#schema-SearchHospitalResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `orgId` | integer (int32) | — | — |  |
| `orgName` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "orgId": 0,
      "orgName": "string"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-hospitalpaymentsetting-finance-hospitalsettingmonitor"></a>
#### 5.1.2 รายการตั้งค่าการจ่ายเงิน

```http
GET /api/HospitalPaymentSetting/Finance/HospitalSettingMonitor
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `searchDetail` | query | string | — |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [HospitalSettingMonitorReponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [HospitalSettingMonitorReponseDto](#schema-HospitalSettingMonitorReponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalPaymentSettingId` | string (uuid) | — | — |  |
| `hospitalName` | string | — | ได้ | สถานพยาบาล |
| `isAutoPay` | boolean | — | ได้ | จ่ายเงินอัตโนมัติ |
| `delayDays` | integer (int32) | — | — | จำนวนวันที่หน่วงการจ่าย |
| `holdStatusId` | integer (int32) | — | ได้ | สถานะ Hold |
| `holdStatusName` | string | — | ได้ |  |
| `updatedDate` | string (date-time) | — | ได้ | วันที่อัปเดตล่าสุด |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "hospitalPaymentSettingId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "hospitalName": "string",
      "isAutoPay": true,
      "delayDays": 0,
      "holdStatusId": 0,
      "holdStatusName": "string",
      "updatedDate": "2026-10-09T10:00:00Z"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z",
  "totalAmountRecords": 100.0,
  "totalAmountPages": 10.0,
  "currentPage": 1.0,
  "recordsPerPage": 10.0,
  "pageIndex": 1
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-hospitalpaymentsetting-finance-addhospitalsetting"></a>
#### 5.1.3 เพิ่มสถานพยาบาลที่ต้องการตั้งค่า

```http
POST /api/HospitalPaymentSetting/Finance/AddHospitalSetting
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [AddHospitalSettingRequestDto](#schema-AddHospitalSettingRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalId` | integer (int32) | — | — |  |
| `hospitalName` | string | ✅ | — |  |
| `autoPayDelayDays` | integer (int32) | — | — |  |
| `holdStatusId` | integer (int32) | — | — |  |
| `isAutoPay` | boolean | — | — |  |

ตัวอย่าง:

```json
{
  "hospitalId": 0,
  "hospitalName": "string",
  "autoPayDelayDays": 0,
  "holdStatusId": 0,
  "isAutoPay": true
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [AddHospitalSettingResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [AddHospitalSettingResponseDto](#schema-AddHospitalSettingResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "isSuccess": true,
    "message": "string"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-hospitalpaymentsetting-finance-updatehospitalsetting"></a>
#### 5.1.4 แก้ไข สถานพยาบาลที่ต้องการตั้งค่า

```http
POST /api/HospitalPaymentSetting/Finance/UpdateHospitalSetting
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [UpdateHospitalSettingRequestDto](#schema-UpdateHospitalSettingRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalPaymentSettingId` | string (uuid) | — | — |  |
| `autoPayDelayDays` | integer (int32) | — | — |  |
| `holdStatusId` | integer (int32) | — | — |  |
| `isAutoPay` | boolean | — | — |  |

ตัวอย่าง:

```json
{
  "hospitalPaymentSettingId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "autoPayDelayDays": 0,
  "holdStatusId": 0,
  "isAutoPay": true
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [UpdateHospitalSettingResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [UpdateHospitalSettingResponseDto](#schema-UpdateHospitalSettingResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "isSuccess": true,
    "message": "string"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-hospitalpaymentsetting-finance-gethistories"></a>
#### 5.1.5 ประวัติการตั้งค่า

```http
GET /api/HospitalPaymentSetting/Finance/GetHistories
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `hospitalPaymentSettingId` | query | string (uuid) | — |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [HistoryResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [HistoryResponseDto](#schema-HistoryResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `actionName` | string | — | ได้ |  |
| `createdDate` | string (date-time) | — | ได้ |  |
| `createdByUser` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "actionName": "string",
      "createdDate": "2026-10-09T10:00:00Z",
      "createdByUser": "string"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="group-hospitaltransfer"></a>
### 5.2 การโอนเงินโรงพยาบาล (`HospitalTransfer`)

จัดกลุ่มรายการจ่ายเงิน ส่งโอน ติดตามสถานะ และส่งอีเมลแจ้งผลการโอนให้โรงพยาบาล

<a id="ep-post-api-hospitaltransfer-v1-hospitaltransfermonitor"></a>
#### 5.2.1 รายการรอกรุ๊ปโอนเงินโรงพยาบาล

```http
POST /api/HospitalTransfer/v1/HospitalTransferMonitor
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `Page` | query | integer (int32) | — |  |
| `RecordsPerPage` | query | integer (int32) | — |  |

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [HospitalTransferMonitorFilterDto](#schema-HospitalTransferMonitorFilterDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `statusId` | integer (int32) | — | ได้ | สถานะรายการ 2 = รอสร้างรายการ (ไม่บังคับ) |
| `hospitalName` | string | — | ได้ | ชื่อสถานพยาบาล (ไม่บังคับ) ถ้าไม่ส่งจะแสดงข้อมูลทั้งหมด |

ตัวอย่าง:

```json
{
  "statusId": 0,
  "hospitalName": "string"
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [HospitalTransferGroupMonitorResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [HospitalTransferGroupMonitorResponseDto](#schema-HospitalTransferGroupMonitorResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `transferGroupId` | string (uuid) | — | — | รหัสกรุ๊ปโอนเงิน |
| `transferGroupNo` | string | — | ได้ | เลขอ้างอิงกรุ๊ปโอนเงิน |
| `insuranceCompanyCode` | string | — | ได้ | รหัสบริษัทประกัน |
| `insuranceCompanyNameTH` | string | — | ได้ | ชื่อบริษัทประกัน |
| `branchCode` | string | — | ได้ | รหัสสาขา |
| `branchNameTH` | string | — | ได้ | ชื่อสาขา |
| `billControlNo` | string | — | ได้ | เลขอ้างอิงใบคุม รพ. |
| `caseId` | string (uuid) | — | — | รหัสเคส |
| `caseNo` | string | — | ได้ | เลขที่อ้างอิงเคส |
| `billSentDate` | string (date-time) | — | ได้ | วันที่ส่งวางบิล |
| `insuredName` | string | — | ได้ | ชื่อผู้เอาประกัน |
| `hospitalId` | integer (int32) | — | — | รหัสสถานพยาบาล |
| `hospitalName` | string | — | ได้ | สถานพยาบาล |
| `amount` | number (double) | — | ได้ | จำนวนเงิน |
| `statusId` | integer (int32) | — | ได้ | รหัสสถานะรายการ |
| `statusNameTH` | string | — | ได้ | ชื่อสถานะรายการ |
| `statusCode` | string | — | ได้ | รหัสสถานะสำหรับแสดงผล เช่น waiting-create |
| `isSelectable` | boolean | — | — | เลือกรายการเพื่อสร้างรายการโอนเงินได้หรือไม่ |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "transferGroupId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "transferGroupNo": "string",
      "insuranceCompanyCode": "string",
      "insuranceCompanyNameTH": "string",
      "branchCode": "string",
      "branchNameTH": "string",
      "billControlNo": "string",
      "caseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "caseNo": "string",
      "billSentDate": "2026-10-09T10:00:00Z",
      "insuredName": "string",
      "hospitalId": 0,
      "hospitalName": "string",
      "amount": 0.0,
      "statusId": 0,
      "statusNameTH": "string",
      "statusCode": "string",
      "isSelectable": true
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z",
  "totalAmountRecords": 100.0,
  "totalAmountPages": 10.0,
  "currentPage": 1.0,
  "recordsPerPage": 10.0,
  "pageIndex": 1
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-hospitaltransfer-v1-hospitaltransferstatusmonitor"></a>
#### 5.2.2 สถานะการโอนเงินโรงพยาบาล

```http
POST /api/HospitalTransfer/v1/HospitalTransferStatusMonitor
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `Page` | query | integer (int32) | — |  |
| `RecordsPerPage` | query | integer (int32) | — |  |

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [HospitalTransferMonitorFilterDto](#schema-HospitalTransferMonitorFilterDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `statusId` | integer (int32) | — | ได้ | สถานะรายการ 2 = รอสร้างรายการ (ไม่บังคับ) |
| `hospitalName` | string | — | ได้ | ชื่อสถานพยาบาล (ไม่บังคับ) ถ้าไม่ส่งจะแสดงข้อมูลทั้งหมด |

ตัวอย่าง:

```json
{
  "statusId": 0,
  "hospitalName": "string"
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [HospitalTransferStatusMonitorResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [HospitalTransferStatusMonitorResponseDto](#schema-HospitalTransferStatusMonitorResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentId` | string (uuid) | — | — | รหัสรายการโอนเงิน |
| `paymentCode` | string | — | ได้ | เลขอ้างอิงการโอน |
| `paymentDate` | string (date-time) | — | ได้ | วันที่สร้างรายการ |
| `expectedPaymentDate` | string (date-time) | — | ได้ | วันที่คาดว่าเงินจะออก |
| `hospitalId` | integer (int32) | — | — | รหัสสถานพยาบาล |
| `hospitalName` | string | — | ได้ | สถานพยาบาล |
| `itemCount` | integer (int32) | — | — | จำนวนราย |
| `amount` | number (double) | — | ได้ | จำนวนเงิน |
| `netAmount` | number (double) | — | ได้ | จำนวนเงินสุทธิ (หัก 3%) |
| `toBank` | string | — | ได้ | ธนาคารผู้รับเงิน |
| `toAccountNo` | string | — | ได้ | เลขที่บัญชีผู้รับเงิน |
| `toAccountName` | string | — | ได้ | ชื่อบัญชีผู้รับเงิน |
| `statusId` | integer (int32) | — | ได้ | รหัสสถานะรายการ |
| `statusNameTH` | string | — | ได้ | ชื่อสถานะรายการ |
| `statusCode` | string | — | ได้ | รหัสสถานะสำหรับแสดงผล เช่น waiting-transfer |
| `isSelectable` | boolean | — | — | เลือกรายการเพื่อดำเนินการต่อได้หรือไม่ |
| `actionId` | string (uuid) | — | — | รหัสรายการสำหรับดำเนินการ |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "paymentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "paymentCode": "string",
      "paymentDate": "2026-10-09T10:00:00Z",
      "expectedPaymentDate": "2026-10-09T10:00:00Z",
      "hospitalId": 0,
      "hospitalName": "string",
      "itemCount": 0,
      "amount": 0.0,
      "netAmount": 0.0,
      "toBank": "string",
      "toAccountNo": "string",
      "toAccountName": "string",
      "statusId": 0,
      "statusNameTH": "string",
      "statusCode": "string",
      "isSelectable": true,
      "actionId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z",
  "totalAmountRecords": 100.0,
  "totalAmountPages": 10.0,
  "currentPage": 1.0,
  "recordsPerPage": 10.0,
  "pageIndex": 1
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-hospitaltransfer-v1-hospitaltransferdetail"></a>
#### 5.2.3 รายละเอียดรายการโอนเงินโรงพยาบาล

```http
GET /api/HospitalTransfer/v1/HospitalTransferDetail
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `transferId` | query | string (uuid) | — |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [HospitalTransferDetailResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [HospitalTransferDetailResponseDto](#schema-HospitalTransferDetailResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `transferId` | string (uuid) | — | — |  |
| `paymentCode` | string | — | ได้ |  |
| `statusId` | integer (int32) | — | — |  |
| `statusNameTH` | string | — | ได้ |  |
| `statusCode` | string | — | ได้ |  |
| `hospitalName` | string | — | ได้ |  |
| `toBank` | string | — | ได้ |  |
| `toAccountNo` | string | — | ได้ |  |
| `toAccountName` | string | — | ได้ |  |
| `paymentDate` | string (date-time) | — | ได้ |  |
| `expectedPaymentDate` | string (date-time) | — | ได้ |  |
| `totalGrossPaidAmount` | number (double) | — | — |  |
| `totalWithHoldingTaxAmount` | number (double) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | — |  |
| `isSendEmail` | boolean | — | ได้ |  |
| `details` | array&lt;[HospitalTransferDetailItemDto](#schema-HospitalTransferDetailItemDto)&gt; | — | ได้ |  |
| `details[].sequenceNo` | integer (int32) | — | — |  |
| `details[].caseNo` | string | — | ได้ |  |
| `details[].insuredName` | string | — | ได้ |  |
| `details[].grossPaidAmount` | number (double) | — | — |  |
| `details[].withHoldingTaxAmount` | number (double) | — | — |  |
| `details[].netPaidAmount` | number (double) | — | — |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "transferId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "paymentCode": "string",
    "statusId": 0,
    "statusNameTH": "string",
    "statusCode": "string",
    "hospitalName": "string",
    "toBank": "string",
    "toAccountNo": "string",
    "toAccountName": "string",
    "paymentDate": "2026-10-09T10:00:00Z",
    "expectedPaymentDate": "2026-10-09T10:00:00Z",
    "totalGrossPaidAmount": 0.0,
    "totalWithHoldingTaxAmount": 0.0,
    "totalNetPaidAmount": 0.0,
    "isSendEmail": true,
    "details": [
      {
        "sequenceNo": 0,
        "caseNo": "string",
        "insuredName": "string",
        "grossPaidAmount": 0.0,
        "withHoldingTaxAmount": 0.0,
        "netPaidAmount": 0.0
      }
    ]
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-hospitaltransfer-v1-transferhospital"></a>
#### 5.2.4 ส่งรายการจ่ายเงินโรงพยาบาลไปยังระบบโอนเงิน

```http
POST /api/HospitalTransfer/v1/TransferHospital
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** — รายการจ่ายเงินโรงพยาบาลที่ต้องการโอน (จำเป็น)

`Content-Type: application/json`

Schema: [TransferHospitalRequestDto](#schema-TransferHospitalRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentId` | string (uuid) | — | — | รหัสรายการจ่ายเงินโรงพยาบาลที่ต้องการโอน |

ตัวอย่าง:

```json
{
  "paymentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [CreateTransferResponseDTOServiceResponse](#conventions) — `data` เป็น object ของ [CreateTransferResponseDTO](#schema-CreateTransferResponseDTO)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `itemCout` | integer (int32) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | ได้ |  |
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |
| `payListHeaderId` | string (uuid) | — | ได้ |  |
| `refTransactionId` | string (uuid) | — | ได้ |  |
| `paymentItemCount` | integer (int32) | — | — |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "itemCout": 0,
    "totalNetPaidAmount": 0.0,
    "isSuccess": true,
    "message": "string",
    "payListHeaderId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "refTransactionId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "paymentItemCount": 0
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-hospitaltransfer-v1-createpaymenthospital"></a>
#### 5.2.5 สร้างรายการ กรุ๊ปโอนเงินโรงพยาบาล ให้เป็นสถานะรอโอน

```http
POST /api/HospitalTransfer/v1/CreatePaymentHospital
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** (จำเป็น)

`Content-Type: application/json`

Body เป็น **array** ของ object ต่อไปนี้ (ส่งได้หลายรายการต่อครั้ง):

Schema: [CreatePaymentHospitalRequestDto](#schema-CreatePaymentHospitalRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalId` | integer (int32) | ✅ | — |  |
| `transferId` | string (uuid) | ✅ | — |  |

ตัวอย่าง:

```json
[
  {
    "hospitalId": 0,
    "transferId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  }
]
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [CreatePaymentResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [CreatePaymentResponseDto](#schema-CreatePaymentResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `itemCount` | integer (int32) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | ได้ |  |
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |
| `payListHeaderId` | string (uuid) | — | ได้ |  |
| `refTransactionId` | string (uuid) | — | ได้ |  |
| `paymentCodeResponse` | array&lt;[CreatePaymentCodeResponseDto](#schema-CreatePaymentCodeResponseDto)&gt; | — | ได้ |  |
| `paymentCodeResponse[].hospitalName` | string | — | ได้ |  |
| `paymentCodeResponse[].paymentCode` | string | — | ได้ |  |
| `paymentCodeResponse[].totalCount` | integer (int32) | — | — |  |
| `paymentCodeResponse[].netPaidAmount` | number (double) | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "itemCount": 0,
    "totalNetPaidAmount": 0.0,
    "isSuccess": true,
    "message": "string",
    "payListHeaderId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "refTransactionId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "paymentCodeResponse": [
      {
        "hospitalName": "string",
        "paymentCode": "string",
        "totalCount": 0,
        "netPaidAmount": 0.0
      }
    ]
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-hospitaltransfer-v1-transferhospitalgroup"></a>
#### 5.2.6 โอนเงินโรงพยาบาลแบบกรุ๊ป

```http
POST /api/HospitalTransfer/v1/TransferHospitalGroup
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** (จำเป็น)

`Content-Type: application/json`

Body เป็น **array** ของ object ต่อไปนี้ (ส่งได้หลายรายการต่อครั้ง):

Schema: [HospitalTransferGroupRequestDto](#schema-HospitalTransferGroupRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentId` | string (uuid) | — | — |  |

ตัวอย่าง:

```json
[
  {
    "paymentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  }
]
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [HospitalTransferGroupResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [HospitalTransferGroupResponseDto](#schema-HospitalTransferGroupResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "isSuccess": true,
    "message": "string"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-hospitaltransfer-v1-sendemailtohospital"></a>
#### 5.2.7 ส่ง Email ผลการโอนเงินให้ รพ.

```http
POST /api/HospitalTransfer/v1/SendEmailToHospital
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [SendEmailToHospitalRequestDto](#schema-SendEmailToHospitalRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentId` | string (uuid) | ✅ | — |  |

ตัวอย่าง:

```json
{
  "paymentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [SendEmailResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [SendEmailResponseDto](#schema-SendEmailResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "isSuccess": true,
    "message": "string"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="group-masters"></a>
### 5.3 ข้อมูลหลัก (Master Data) (`Masters`)

ข้อมูลอ้างอิงสำหรับ dropdown/filter เช่น สถานะการโอน สถานะการจ่าย สาเหตุการโอนเพิ่ม และประเภทความสัมพันธ์ของบัญชี

<a id="ep-get-api-masters-getadjustmentreasons"></a>
#### 5.3.1 ข้อมูลสาเหตุการโอนเพิ่ม

```http
GET /api/Masters/GetAdjustmentReasons
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [AdjustmentReasonResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [AdjustmentReasonResponseDto](#schema-AdjustmentReasonResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-masters-getadjustmentreasonbyid"></a>
#### 5.3.2 ข้อมูลสาเหตุการโอนเพิ่ม By Id

```http
GET /api/Masters/GetAdjustmentReasonById
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `AdjustmentReasonId` | query | integer (int32) | — |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [AdjustmentReasonResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [AdjustmentReasonResponseDto](#schema-AdjustmentReasonResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "id": 0,
    "name": "string"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-masters-gettransferstatuses"></a>
#### 5.3.3 ข้อมูลสถานะการโอนเงิน (Transfer Status)

```http
GET /api/Masters/GetTransferStatuses
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [TransferStatusResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [TransferStatusResponseDto](#schema-TransferStatusResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-masters-getpaymentstatuses"></a>
#### 5.3.4 ข้อมูลสถานะการจ่ายเงิน (Payment Status)

```http
GET /api/Masters/GetPaymentStatuses
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [PaymentStatusResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [PaymentStatusResponseDto](#schema-PaymentStatusResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-masters-gethospitaltransfergroupstatuses"></a>
#### 5.3.5 ข้อมูลสถานะรายการของกรุ๊ปโอนเงินโรงพยาบาล (สำหรับ filter หน้าจอค้นหารายการโอนเงิน)

```http
GET /api/Masters/GetHospitalTransferGroupStatuses
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [PaymentStatusResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [PaymentStatusResponseDto](#schema-PaymentStatusResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-masters-getbankaccountrelationtypes"></a>
#### 5.3.6 ข้อมูลประเภทความสัมพันธ์ของบัญชี กับผู้รับสินไหม

```http
GET /api/Masters/GetBankAccountRelationTypes
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [BankAccountRelationTypeResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [BankAccountRelationTypeResponseDto](#schema-BankAccountRelationTypeResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-masters-getbankaccountrelationtypebyid"></a>
#### 5.3.7 ข้อมูลประเภทความสัมพันธ์ของบัญชี กับผู้รับสินไหม By Id

```http
GET /api/Masters/GetBankAccountRelationTypeById
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `bankAccountRelationTypeId` | query | integer (int32) | — |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [BankAccountRelationTypeResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [BankAccountRelationTypeResponseDto](#schema-BankAccountRelationTypeResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "id": 0,
    "name": "string"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="group-notification"></a>
### 5.4 การแจ้งเตือนและแบบประเมิน (`Notification`)

ข้อมูลรายการ SMS และการจัดการแบบประเมินความพึงพอใจ (Survey)

<a id="ep-get-api-notification-gettransactionbyid"></a>
#### 5.4.1 Get Transaction ของรายการส่ง SMS

```http
GET /api/Notification/GetTransactionById
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI` และ Header `apiKey`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `payTransferTransactionId` | query | string (uuid) | — |  |
| `apiKey` | header | string | — |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [GetSMSTransactionResponseServiceResponse](#conventions) — `data` เป็น object ของ [GetSMSTransactionResponse](#schema-GetSMSTransactionResponse)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `smStransactionId` | string (uuid) | — | ได้ |  |
| `surveyId` | integer (int32) | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "smStransactionId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "surveyId": 0
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-notification-updatesmstransactionsurvey"></a>
#### 5.4.2 Update SurveyId ใน SMSTransaction

```http
POST /api/Notification/UpdateSMSTransactionSurvey
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI` และ Header `apiKey`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `apiKey` | header | string | — |  |

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [SMSTransacttionSurveyRequest](#schema-SMSTransacttionSurveyRequest)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `smStransactionId` | string (uuid) | ✅ | — |  |
| `surveyId` | integer (int32) | ✅ | — |  |

ตัวอย่าง:

```json
{
  "smStransactionId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "surveyId": 0
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [SMSTransacttionSurveyResponseServiceResponse](#conventions) — `data` เป็น object ของ [SMSTransacttionSurveyResponse](#schema-SMSTransacttionSurveyResponse)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `smStransactionId` | string (uuid) | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "smStransactionId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-notification-savesurveyfeedback"></a>
#### 5.4.3 ผลตอบแบบประเมิน

```http
POST /api/Notification/SaveSurveyFeedback
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI` และ Header `apiKey`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `apiKey` | header | string | — |  |

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [SaveSurveyFeedbackRequestDto](#schema-SaveSurveyFeedbackRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `surveyId` | integer (int32) | ✅ | — |  |
| `answersRequest` | [JToken](#schema-JToken) | ✅ | — |  |

ตัวอย่าง:

```json
{
  "surveyId": 0,
  "answersRequest": []
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [SaveSurveyFeedbackResponsetDtoServiceResponse](#conventions) — `data` เป็น object ของ [SaveSurveyFeedbackResponsetDto](#schema-SaveSurveyFeedbackResponsetDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `surveyId` | integer (int32) | — | — |  |
| `message` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "surveyId": 0,
    "message": "string"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-notification-getsurveyid"></a>
#### 5.4.4 Get surveyId

```http
GET /api/Notification/GetSurveyId
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI` และ Header `apiKey`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `apiKey` | header | string | — |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [GetSurveyIdResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [GetSurveyIdResponseDto](#schema-GetSurveyIdResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `surveyId` | integer (int32) | — | — |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "surveyId": 0
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="group-remainingbalance"></a>
### 5.5 ยอดเงินคงเหลือ (`RemainingBalance`)

บันทึกและเรียกดูยอดเงินคงเหลือของบัญชีธนาคาร

<a id="ep-post-api-remainingbalance-insertremainingbalance"></a>
#### 5.5.1 Insert ยอดเงินคงเหลือของบัญชี

```http
POST /api/RemainingBalance/InsertRemainingBalance
```

**Authentication:** API Key ผ่าน Header `apiKey`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `apiKey` | header | string | — |  |

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [RemainingBalanceRequestDto](#schema-RemainingBalanceRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `bankId` | integer (int32) | ✅ | — |  |
| `accountNo` | string | ✅ | — |  |
| `accountName` | string | ✅ | — |  |
| `balance` | number (double) | ✅ | — |  |

ตัวอย่าง:

```json
{
  "bankId": 0,
  "accountNo": "string",
  "accountName": "string",
  "balance": 0.0
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |

Response `200`: [RemainingBalanceReponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [RemainingBalanceReponseDto](#schema-RemainingBalanceReponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "isSuccess": true,
    "message": "string"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-remainingbalance-insertremainingbalance"></a>
#### 5.5.2 Get ข้อมูลยอดเงินคงเหลือในบัญชี

```http
GET /api/RemainingBalance/InsertRemainingBalance
```

**Authentication:** ไม่ระบุใน spec

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [GetDataByAccountDetailRequestDto](#schema-GetDataByAccountDetailRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `bankId` | integer (int32) | ✅ | — |  |
| `accountNo` | string | ✅ | — |  |
| `accountName` | string | ✅ | — |  |

ตัวอย่าง:

```json
{
  "bankId": 0,
  "accountNo": "string",
  "accountName": "string"
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |

Response `200`: [GetDataByAccountDetailResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [GetDataByAccountDetailResponseDto](#schema-GetDataByAccountDetailResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `bankId` | integer (int32) | — | — |  |
| `accountNo` | string | — | ได้ |  |
| `accountName` | string | — | ได้ |  |
| `balance` | number (double) | — | — |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "bankId": 0,
    "accountNo": "string",
    "accountName": "string",
    "balance": 0.0
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="group-setting"></a>
### 5.6 การตั้งค่าการโอนเงิน (`Setting`)

ตั้งค่าการโอนเงินอัตโนมัติ และค้นหารายการจากเลขที่ ClaimNo / CaseNo

<a id="ep-get-api-setting-getcurrentsetting"></a>
#### 5.6.1 ตั้งค่าการโอนเงิน

```http
GET /api/Setting/GetCurrentSetting
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [PayTransferSettingResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [PayTransferSettingResponseDto](#schema-PayTransferSettingResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paytransferSettingId` | string (uuid) | — | — |  |
| `isAutoTransfer` | boolean | — | — |  |
| `history` | array&lt;[PayTransferSettingHistoryResponseDto](#schema-PayTransferSettingHistoryResponseDto)&gt; | — | ได้ |  |
| `history[].createdByUser` | string | — | ได้ |  |
| `history[].employeeCode` | string | — | ได้ |  |
| `history[].isAutoTransfer` | boolean | — | — |  |
| `history[].createdDate` | string (date-time) | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "paytransferSettingId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "isAutoTransfer": true,
    "history": [
      {
        "createdByUser": "string",
        "employeeCode": "string",
        "isAutoTransfer": true,
        "createdDate": "2026-10-09T10:00:00Z"
      }
    ]
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-setting-updatesettingautotransfer"></a>
#### 5.6.2 บันทึกการตั้งค่าการโอนเงิน

```http
POST /api/Setting/UpdateSettingAutoTransfer
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [UpdatePayTransferSettingRequestDto](#schema-UpdatePayTransferSettingRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paytransferSettingId` | string (uuid) | ✅ | — |  |

ตัวอย่าง:

```json
{
  "paytransferSettingId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [UpdatePayTransferSettingRequestDtoServiceResponse](#conventions) — `data` เป็น object ของ [UpdatePayTransferSettingRequestDto](#schema-UpdatePayTransferSettingRequestDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paytransferSettingId` | string (uuid) | ✅ | — |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "paytransferSettingId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-setting-searchclaimorcase"></a>
#### 5.6.3 ค้นหารายการจาก เลขที่ ClaimNo / CaseNo

```http
GET /api/Setting/SearchClaimOrCase
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `searchDetail` | query | string | ✅ |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [SearchClaimOrCaseResponseDtoListServiceResponse](#conventions) — `data` เป็น **array** ของ [SearchClaimOrCaseResponseDto](#schema-SearchClaimOrCaseResponseDto)

**ฟิลด์ใน `data`[]**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `caseId` | string (uuid) | — | — |  |
| `claimId` | string (uuid) | — | ได้ |  |
| `claimCase` | string | — | ได้ |  |
| `createdClaimDate` | string (date-time) | — | ได้ | วันที่สร้างเคลม |
| `customerName` | string | — | ได้ |  |
| `coverageType` | string | — | ได้ | ประเภทความคุ้มครอง |
| `caseAmount` | number (double) | — | ได้ | จำนวนเงิน (case) |
| `isClaimNo` | boolean | — | — | รายการที่ค้นหาเป็นเลขที่ ClaimNo หรือไม |

ตัวอย่าง Response:

```json
{
  "data": [
    {
      "caseId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "claimId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "claimCase": "string",
      "createdClaimDate": "2026-10-09T10:00:00Z",
      "customerName": "string",
      "coverageType": "string",
      "caseAmount": 0.0,
      "isClaimNo": true
    }
  ],
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="group-transfer"></a>
### 5.7 การโอนเงิน (`Transfer`)

สร้างรายการโอนเงินค่าสินไหมทดแทน ตรวจสอบรายละเอียด และรับผลการโอน

<a id="ep-post-api-transfer-v1-createtransferresult"></a>
#### 5.7.1 Step 3 CreatedTransfer ทดสอบผลการสร้างรายการโอนเงิน

```http
POST /api/Transfer/v1/CreateTransferResult
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Schema: [PayListResult](#schema-PayListResult)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `refCode` | string (uuid) | — | ได้ |  |
| `isSucceed` | boolean | — | — |  |
| `transRefNo` | string | — | ได้ |  |
| `statusBank` | string | — | ได้ |  |
| `description` | string | — | ได้ |  |
| `descriptionTh` | string | — | ได้ |  |
| `transferDate` | string (date-time) | — | ได้ |  |
| `updatedDate` | string (date-time) | — | ได้ |  |
| `payResultStatusId` | integer (int32) | — | ได้ |  |
| `payResultDescription` | string | — | ได้ |  |
| `transType` | string | — | ได้ |  |

ตัวอย่าง:

```json
{
  "refCode": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "isSucceed": true,
  "transRefNo": "string",
  "statusBank": "string",
  "description": "string",
  "descriptionTh": "string",
  "transferDate": "2026-10-09T10:00:00Z",
  "updatedDate": "2026-10-09T10:00:00Z",
  "payResultStatusId": 0,
  "payResultDescription": "string",
  "transType": "string"
}
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

_ไม่มีการระบุโครงสร้าง Response body ใน spec_

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-post-api-transfer-v1-createpayment"></a>
#### 5.7.2 สร้างรายการโอนเงิน และโอนเงิน

```http
POST /api/Transfer/v1/CreatePayment
```

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Request Body** (ไม่บังคับ)

`Content-Type: application/json`

Body เป็น **array** ของ object ต่อไปนี้ (ส่งได้หลายรายการต่อครั้ง):

Schema: [CreatePaymentRequestDto](#schema-CreatePaymentRequestDto)

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `casePayableId` | string (uuid) | ✅ | — |  |
| `grossPaidAmount` | number (double) | ✅ | — |  |
| `withHoldingTaxAmount` | number (double) | ✅ | — |  |
| `netPaidAmount` | number (double) | ✅ | — |  |
| `receivingBankId` | integer (int32) | ✅ | — |  |
| `receivingBankAccountNo` | string | ✅ | — |  |
| `receivingBankName` | string | ✅ | — |  |
| `receivingAccountName` | string | ✅ | — |  |
| `phoneNumber` | string | ✅ | — |  |
| `claimCase` | string | ✅ | — |  |
| `claimNo` | string | ✅ | — |  |
| `payeeTypeId` | integer (int32) | — | — |  |
| `paymentTypeId` | integer (int32) | — | — |  |
| `userId` | integer (int32) | — | — |  |

ตัวอย่าง:

```json
[
  {
    "casePayableId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "grossPaidAmount": 0.0,
    "withHoldingTaxAmount": 0.0,
    "netPaidAmount": 0.0,
    "receivingBankId": 0,
    "receivingBankAccountNo": "string",
    "receivingBankName": "string",
    "receivingAccountName": "string",
    "phoneNumber": "0812345678",
    "claimCase": "string",
    "claimNo": "string",
    "payeeTypeId": 0,
    "paymentTypeId": 0,
    "userId": 0
  }
]
```

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [CreatePaymentResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [CreatePaymentResponseDto](#schema-CreatePaymentResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `itemCount` | integer (int32) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | ได้ |  |
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |
| `payListHeaderId` | string (uuid) | — | ได้ |  |
| `refTransactionId` | string (uuid) | — | ได้ |  |
| `paymentCodeResponse` | array&lt;[CreatePaymentCodeResponseDto](#schema-CreatePaymentCodeResponseDto)&gt; | — | ได้ |  |
| `paymentCodeResponse[].hospitalName` | string | — | ได้ |  |
| `paymentCodeResponse[].paymentCode` | string | — | ได้ |  |
| `paymentCodeResponse[].totalCount` | integer (int32) | — | — |  |
| `paymentCodeResponse[].netPaidAmount` | number (double) | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "itemCount": 0,
    "totalNetPaidAmount": 0.0,
    "isSuccess": true,
    "message": "string",
    "payListHeaderId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "refTransactionId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "paymentCodeResponse": [
      {
        "hospitalName": "string",
        "paymentCode": "string",
        "totalCount": 0,
        "netPaidAmount": 0.0
      }
    ]
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="ep-get-api-transfer-v1-paymentdetails"></a>
#### 5.7.3 รายละเอียดรายการชำระเงิน (ตาม referenceCode)

```http
GET /api/Transfer/v1/PaymentDetails
```

> ℹ️ Endpoint นี้ไม่มี `summary` ใน spec ต้นฉบับ ชื่อข้างต้นตั้งโดยผู้จัดทำเอกสารจากชื่อ Endpoint และ Response

**Authentication:** OAuth 2.0 — scope `ClaimFundAPI`

**Parameters**

| ชื่อ | ตำแหน่ง | ชนิดข้อมูล | จำเป็น | คำอธิบาย |
|---|:---:|---|:---:|---|
| `referenceCode` | query | string (uuid) | ✅ |  |

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |
| `401` | Unauthorized |
| `403` | Forbidden |

Response `200`: [PaymentTransactionResponseDtoServiceResponse](#conventions) — `data` เป็น object ของ [PaymentTransactionResponseDto](#schema-PaymentTransactionResponseDto)

**ฟิลด์ใน `data`**

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentDate` | string (date-time) | — | ได้ |  |
| `printDate` | string (date-time) | — | ได้ |  |
| `admissionDate` | string (date-time) | — | ได้ |  |
| `countItem` | integer (int32) | — | — |  |
| `paymentId` | string (uuid) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | ได้ |  |
| `customerName` | string | — | ได้ |  |
| `employee` | string | — | ได้ |  |
| `accountNo` | string | — | ได้ |  |
| `accountName` | string | — | ได้ |  |
| `bankName` | string | — | ได้ |  |
| `paymentCode` | string | — | ได้ |  |
| `transRefNo` | string | — | ได้ |  |
| `paymentItemDetail` | array&lt;[PaymentItemDetailDto](#schema-PaymentItemDetailDto)&gt; | — | ได้ |  |
| `paymentItemDetail[].claimNo` | string | — | ได้ |  |
| `paymentItemDetail[].customerName` | string | — | ได้ |  |
| `paymentItemDetail[].transactionAmount` | number (double) | — | ได้ |  |
| `paymentItemDetail[].bankName` | string | — | ได้ |  |
| `paymentItemDetail[].approvedAmount` | number (double) | — | ได้ |  |

ตัวอย่าง Response:

```json
{
  "data": {
    "paymentDate": "2026-10-09T10:00:00Z",
    "printDate": "2026-10-09T10:00:00Z",
    "admissionDate": "2026-10-09T10:00:00Z",
    "countItem": 0,
    "paymentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "totalNetPaidAmount": 0.0,
    "customerName": "string",
    "employee": "string",
    "accountNo": "string",
    "accountName": "string",
    "bankName": "string",
    "paymentCode": "string",
    "transRefNo": "string",
    "paymentItemDetail": [
      {
        "claimNo": "string",
        "customerName": "string",
        "transactionAmount": 0.0,
        "bankName": "string",
        "approvedAmount": 0.0
      }
    ]
  },
  "isSuccess": true,
  "message": null,
  "code": 200,
  "exceptionMessage": null,
  "serverDateTime": "2026-10-09T10:00:00Z"
}
```

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="group-versioncontrol"></a>
### 5.8 เวอร์ชันแอปพลิเคชัน (`VersionControl`)

ตรวจสอบเวอร์ชันของแอปพลิเคชัน

<a id="ep-get-version-getapplicationversioncontrol"></a>
#### 5.8.1 Version ของ Application

```http
GET /Version/GetApplicationVersionControl
```

**Authentication:** ไม่ระบุใน spec

**Responses**

| Status | คำอธิบาย |
|:---:|---|
| `200` | Success |

_ไม่มีการระบุโครงสร้าง Response body ใน spec_

[⬆ กลับสู่สารบัญ](#toc)

---

<a id="schemas"></a>
## 6. Data Models (Schemas)

Schema ที่เป็น `*ServiceResponse` (Envelope) ไม่แสดงซ้ำ — ดูโครงสร้างได้ที่ [หัวข้อ 3.2](#conventions)

| Schema | จำนวนฟิลด์ |
|---|:---:|
| [AddHospitalSettingRequestDto](#schema-AddHospitalSettingRequestDto) | 5 |
| [AddHospitalSettingResponseDto](#schema-AddHospitalSettingResponseDto) | 2 |
| [AdjustmentReasonResponseDto](#schema-AdjustmentReasonResponseDto) | 2 |
| [BankAccountRelationTypeResponseDto](#schema-BankAccountRelationTypeResponseDto) | 2 |
| [CreatePaymentCodeResponseDto](#schema-CreatePaymentCodeResponseDto) | 4 |
| [CreatePaymentHospitalRequestDto](#schema-CreatePaymentHospitalRequestDto) | 2 |
| [CreatePaymentRequestDto](#schema-CreatePaymentRequestDto) | 14 |
| [CreatePaymentResponseDto](#schema-CreatePaymentResponseDto) | 7 |
| [CreateTransferResponseDTO](#schema-CreateTransferResponseDTO) | 7 |
| [GetDataByAccountDetailRequestDto](#schema-GetDataByAccountDetailRequestDto) | 3 |
| [GetDataByAccountDetailResponseDto](#schema-GetDataByAccountDetailResponseDto) | 4 |
| [GetSMSTransactionResponse](#schema-GetSMSTransactionResponse) | 2 |
| [GetSurveyIdResponseDto](#schema-GetSurveyIdResponseDto) | 1 |
| [HistoryResponseDto](#schema-HistoryResponseDto) | 3 |
| [HospitalSettingMonitorReponseDto](#schema-HospitalSettingMonitorReponseDto) | 7 |
| [HospitalTransferDetailItemDto](#schema-HospitalTransferDetailItemDto) | 6 |
| [HospitalTransferDetailResponseDto](#schema-HospitalTransferDetailResponseDto) | 16 |
| [HospitalTransferGroupMonitorResponseDto](#schema-HospitalTransferGroupMonitorResponseDto) | 18 |
| [HospitalTransferGroupRequestDto](#schema-HospitalTransferGroupRequestDto) | 1 |
| [HospitalTransferGroupResponseDto](#schema-HospitalTransferGroupResponseDto) | 2 |
| [HospitalTransferMonitorFilterDto](#schema-HospitalTransferMonitorFilterDto) | 2 |
| [HospitalTransferStatusMonitorResponseDto](#schema-HospitalTransferStatusMonitorResponseDto) | 17 |
| [JToken](#schema-JToken) | - |
| [PayListResult](#schema-PayListResult) | 11 |
| [PayTransferSettingHistoryResponseDto](#schema-PayTransferSettingHistoryResponseDto) | 4 |
| [PayTransferSettingResponseDto](#schema-PayTransferSettingResponseDto) | 3 |
| [PaymentItemDetailDto](#schema-PaymentItemDetailDto) | 5 |
| [PaymentStatusResponseDto](#schema-PaymentStatusResponseDto) | 2 |
| [PaymentTransactionResponseDto](#schema-PaymentTransactionResponseDto) | 14 |
| [RemainingBalanceReponseDto](#schema-RemainingBalanceReponseDto) | 2 |
| [RemainingBalanceRequestDto](#schema-RemainingBalanceRequestDto) | 4 |
| [SMSTransacttionSurveyRequest](#schema-SMSTransacttionSurveyRequest) | 2 |
| [SMSTransacttionSurveyResponse](#schema-SMSTransacttionSurveyResponse) | 1 |
| [SaveSurveyFeedbackRequestDto](#schema-SaveSurveyFeedbackRequestDto) | 2 |
| [SaveSurveyFeedbackResponsetDto](#schema-SaveSurveyFeedbackResponsetDto) | 2 |
| [SearchClaimOrCaseResponseDto](#schema-SearchClaimOrCaseResponseDto) | 8 |
| [SearchHospitalResponseDto](#schema-SearchHospitalResponseDto) | 2 |
| [SendEmailResponseDto](#schema-SendEmailResponseDto) | 2 |
| [SendEmailToHospitalRequestDto](#schema-SendEmailToHospitalRequestDto) | 1 |
| [TransferHospitalRequestDto](#schema-TransferHospitalRequestDto) | 1 |
| [TransferStatusResponseDto](#schema-TransferStatusResponseDto) | 2 |
| [UpdateHospitalSettingRequestDto](#schema-UpdateHospitalSettingRequestDto) | 4 |
| [UpdateHospitalSettingResponseDto](#schema-UpdateHospitalSettingResponseDto) | 2 |
| [UpdatePayTransferSettingRequestDto](#schema-UpdatePayTransferSettingRequestDto) | 1 |

<a id="schema-AddHospitalSettingRequestDto"></a>
### `AddHospitalSettingRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalId` | integer (int32) | — | — |  |
| `hospitalName` | string | ✅ | — |  |
| `autoPayDelayDays` | integer (int32) | — | — |  |
| `holdStatusId` | integer (int32) | — | — |  |
| `isAutoPay` | boolean | — | — |  |

<a id="schema-AddHospitalSettingResponseDto"></a>
### `AddHospitalSettingResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

<a id="schema-AdjustmentReasonResponseDto"></a>
### `AdjustmentReasonResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

<a id="schema-BankAccountRelationTypeResponseDto"></a>
### `BankAccountRelationTypeResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

<a id="schema-CreatePaymentCodeResponseDto"></a>
### `CreatePaymentCodeResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalName` | string | — | ได้ |  |
| `paymentCode` | string | — | ได้ |  |
| `totalCount` | integer (int32) | — | — |  |
| `netPaidAmount` | number (double) | — | ได้ |  |

<a id="schema-CreatePaymentHospitalRequestDto"></a>
### `CreatePaymentHospitalRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalId` | integer (int32) | ✅ | — |  |
| `transferId` | string (uuid) | ✅ | — |  |

<a id="schema-CreatePaymentRequestDto"></a>
### `CreatePaymentRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `casePayableId` | string (uuid) | ✅ | — |  |
| `grossPaidAmount` | number (double) | ✅ | — |  |
| `withHoldingTaxAmount` | number (double) | ✅ | — |  |
| `netPaidAmount` | number (double) | ✅ | — |  |
| `receivingBankId` | integer (int32) | ✅ | — |  |
| `receivingBankAccountNo` | string | ✅ | — |  |
| `receivingBankName` | string | ✅ | — |  |
| `receivingAccountName` | string | ✅ | — |  |
| `phoneNumber` | string | ✅ | — |  |
| `claimCase` | string | ✅ | — |  |
| `claimNo` | string | ✅ | — |  |
| `payeeTypeId` | integer (int32) | — | — |  |
| `paymentTypeId` | integer (int32) | — | — |  |
| `userId` | integer (int32) | — | — |  |

<a id="schema-CreatePaymentResponseDto"></a>
### `CreatePaymentResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `itemCount` | integer (int32) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | ได้ |  |
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |
| `payListHeaderId` | string (uuid) | — | ได้ |  |
| `refTransactionId` | string (uuid) | — | ได้ |  |
| `paymentCodeResponse` | array&lt;[CreatePaymentCodeResponseDto](#schema-CreatePaymentCodeResponseDto)&gt; | — | ได้ |  |
| `paymentCodeResponse[].hospitalName` | string | — | ได้ |  |
| `paymentCodeResponse[].paymentCode` | string | — | ได้ |  |
| `paymentCodeResponse[].totalCount` | integer (int32) | — | — |  |
| `paymentCodeResponse[].netPaidAmount` | number (double) | — | ได้ |  |

<a id="schema-CreateTransferResponseDTO"></a>
### `CreateTransferResponseDTO`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `itemCout` | integer (int32) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | ได้ |  |
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |
| `payListHeaderId` | string (uuid) | — | ได้ |  |
| `refTransactionId` | string (uuid) | — | ได้ |  |
| `paymentItemCount` | integer (int32) | — | — |  |

<a id="schema-GetDataByAccountDetailRequestDto"></a>
### `GetDataByAccountDetailRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `bankId` | integer (int32) | ✅ | — |  |
| `accountNo` | string | ✅ | — |  |
| `accountName` | string | ✅ | — |  |

<a id="schema-GetDataByAccountDetailResponseDto"></a>
### `GetDataByAccountDetailResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `bankId` | integer (int32) | — | — |  |
| `accountNo` | string | — | ได้ |  |
| `accountName` | string | — | ได้ |  |
| `balance` | number (double) | — | — |  |

<a id="schema-GetSMSTransactionResponse"></a>
### `GetSMSTransactionResponse`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `smStransactionId` | string (uuid) | — | ได้ |  |
| `surveyId` | integer (int32) | — | ได้ |  |

<a id="schema-GetSurveyIdResponseDto"></a>
### `GetSurveyIdResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `surveyId` | integer (int32) | — | — |  |

<a id="schema-HistoryResponseDto"></a>
### `HistoryResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `actionName` | string | — | ได้ |  |
| `createdDate` | string (date-time) | — | ได้ |  |
| `createdByUser` | string | — | ได้ |  |

<a id="schema-HospitalSettingMonitorReponseDto"></a>
### `HospitalSettingMonitorReponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalPaymentSettingId` | string (uuid) | — | — |  |
| `hospitalName` | string | — | ได้ | สถานพยาบาล |
| `isAutoPay` | boolean | — | ได้ | จ่ายเงินอัตโนมัติ |
| `delayDays` | integer (int32) | — | — | จำนวนวันที่หน่วงการจ่าย |
| `holdStatusId` | integer (int32) | — | ได้ | สถานะ Hold |
| `holdStatusName` | string | — | ได้ |  |
| `updatedDate` | string (date-time) | — | ได้ | วันที่อัปเดตล่าสุด |

<a id="schema-HospitalTransferDetailItemDto"></a>
### `HospitalTransferDetailItemDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `sequenceNo` | integer (int32) | — | — |  |
| `caseNo` | string | — | ได้ |  |
| `insuredName` | string | — | ได้ |  |
| `grossPaidAmount` | number (double) | — | — |  |
| `withHoldingTaxAmount` | number (double) | — | — |  |
| `netPaidAmount` | number (double) | — | — |  |

<a id="schema-HospitalTransferDetailResponseDto"></a>
### `HospitalTransferDetailResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `transferId` | string (uuid) | — | — |  |
| `paymentCode` | string | — | ได้ |  |
| `statusId` | integer (int32) | — | — |  |
| `statusNameTH` | string | — | ได้ |  |
| `statusCode` | string | — | ได้ |  |
| `hospitalName` | string | — | ได้ |  |
| `toBank` | string | — | ได้ |  |
| `toAccountNo` | string | — | ได้ |  |
| `toAccountName` | string | — | ได้ |  |
| `paymentDate` | string (date-time) | — | ได้ |  |
| `expectedPaymentDate` | string (date-time) | — | ได้ |  |
| `totalGrossPaidAmount` | number (double) | — | — |  |
| `totalWithHoldingTaxAmount` | number (double) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | — |  |
| `isSendEmail` | boolean | — | ได้ |  |
| `details` | array&lt;[HospitalTransferDetailItemDto](#schema-HospitalTransferDetailItemDto)&gt; | — | ได้ |  |
| `details[].sequenceNo` | integer (int32) | — | — |  |
| `details[].caseNo` | string | — | ได้ |  |
| `details[].insuredName` | string | — | ได้ |  |
| `details[].grossPaidAmount` | number (double) | — | — |  |
| `details[].withHoldingTaxAmount` | number (double) | — | — |  |
| `details[].netPaidAmount` | number (double) | — | — |  |

<a id="schema-HospitalTransferGroupMonitorResponseDto"></a>
### `HospitalTransferGroupMonitorResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `transferGroupId` | string (uuid) | — | — | รหัสกรุ๊ปโอนเงิน |
| `transferGroupNo` | string | — | ได้ | เลขอ้างอิงกรุ๊ปโอนเงิน |
| `insuranceCompanyCode` | string | — | ได้ | รหัสบริษัทประกัน |
| `insuranceCompanyNameTH` | string | — | ได้ | ชื่อบริษัทประกัน |
| `branchCode` | string | — | ได้ | รหัสสาขา |
| `branchNameTH` | string | — | ได้ | ชื่อสาขา |
| `billControlNo` | string | — | ได้ | เลขอ้างอิงใบคุม รพ. |
| `caseId` | string (uuid) | — | — | รหัสเคส |
| `caseNo` | string | — | ได้ | เลขที่อ้างอิงเคส |
| `billSentDate` | string (date-time) | — | ได้ | วันที่ส่งวางบิล |
| `insuredName` | string | — | ได้ | ชื่อผู้เอาประกัน |
| `hospitalId` | integer (int32) | — | — | รหัสสถานพยาบาล |
| `hospitalName` | string | — | ได้ | สถานพยาบาล |
| `amount` | number (double) | — | ได้ | จำนวนเงิน |
| `statusId` | integer (int32) | — | ได้ | รหัสสถานะรายการ |
| `statusNameTH` | string | — | ได้ | ชื่อสถานะรายการ |
| `statusCode` | string | — | ได้ | รหัสสถานะสำหรับแสดงผล เช่น waiting-create |
| `isSelectable` | boolean | — | — | เลือกรายการเพื่อสร้างรายการโอนเงินได้หรือไม่ |

<a id="schema-HospitalTransferGroupRequestDto"></a>
### `HospitalTransferGroupRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentId` | string (uuid) | — | — |  |

<a id="schema-HospitalTransferGroupResponseDto"></a>
### `HospitalTransferGroupResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

<a id="schema-HospitalTransferMonitorFilterDto"></a>
### `HospitalTransferMonitorFilterDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `statusId` | integer (int32) | — | ได้ | สถานะรายการ 2 = รอสร้างรายการ (ไม่บังคับ) |
| `hospitalName` | string | — | ได้ | ชื่อสถานพยาบาล (ไม่บังคับ) ถ้าไม่ส่งจะแสดงข้อมูลทั้งหมด |

<a id="schema-HospitalTransferStatusMonitorResponseDto"></a>
### `HospitalTransferStatusMonitorResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentId` | string (uuid) | — | — | รหัสรายการโอนเงิน |
| `paymentCode` | string | — | ได้ | เลขอ้างอิงการโอน |
| `paymentDate` | string (date-time) | — | ได้ | วันที่สร้างรายการ |
| `expectedPaymentDate` | string (date-time) | — | ได้ | วันที่คาดว่าเงินจะออก |
| `hospitalId` | integer (int32) | — | — | รหัสสถานพยาบาล |
| `hospitalName` | string | — | ได้ | สถานพยาบาล |
| `itemCount` | integer (int32) | — | — | จำนวนราย |
| `amount` | number (double) | — | ได้ | จำนวนเงิน |
| `netAmount` | number (double) | — | ได้ | จำนวนเงินสุทธิ (หัก 3%) |
| `toBank` | string | — | ได้ | ธนาคารผู้รับเงิน |
| `toAccountNo` | string | — | ได้ | เลขที่บัญชีผู้รับเงิน |
| `toAccountName` | string | — | ได้ | ชื่อบัญชีผู้รับเงิน |
| `statusId` | integer (int32) | — | ได้ | รหัสสถานะรายการ |
| `statusNameTH` | string | — | ได้ | ชื่อสถานะรายการ |
| `statusCode` | string | — | ได้ | รหัสสถานะสำหรับแสดงผล เช่น waiting-transfer |
| `isSelectable` | boolean | — | — | เลือกรายการเพื่อดำเนินการต่อได้หรือไม่ |
| `actionId` | string (uuid) | — | — | รหัสรายการสำหรับดำเนินการ |

<a id="schema-JToken"></a>
### `JToken`

ชนิดข้อมูล JSON อิสระ (Newtonsoft `JToken`) — ใน spec ถูกนิยามเป็น array ที่ซ้อนตัวเองได้ ใช้ใน `SaveSurveyFeedbackRequestDto.answersRequest`

<a id="schema-PayListResult"></a>
### `PayListResult`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `refCode` | string (uuid) | — | ได้ |  |
| `isSucceed` | boolean | — | — |  |
| `transRefNo` | string | — | ได้ |  |
| `statusBank` | string | — | ได้ |  |
| `description` | string | — | ได้ |  |
| `descriptionTh` | string | — | ได้ |  |
| `transferDate` | string (date-time) | — | ได้ |  |
| `updatedDate` | string (date-time) | — | ได้ |  |
| `payResultStatusId` | integer (int32) | — | ได้ |  |
| `payResultDescription` | string | — | ได้ |  |
| `transType` | string | — | ได้ |  |

<a id="schema-PayTransferSettingHistoryResponseDto"></a>
### `PayTransferSettingHistoryResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `createdByUser` | string | — | ได้ |  |
| `employeeCode` | string | — | ได้ |  |
| `isAutoTransfer` | boolean | — | — |  |
| `createdDate` | string (date-time) | — | ได้ |  |

<a id="schema-PayTransferSettingResponseDto"></a>
### `PayTransferSettingResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paytransferSettingId` | string (uuid) | — | — |  |
| `isAutoTransfer` | boolean | — | — |  |
| `history` | array&lt;[PayTransferSettingHistoryResponseDto](#schema-PayTransferSettingHistoryResponseDto)&gt; | — | ได้ |  |
| `history[].createdByUser` | string | — | ได้ |  |
| `history[].employeeCode` | string | — | ได้ |  |
| `history[].isAutoTransfer` | boolean | — | — |  |
| `history[].createdDate` | string (date-time) | — | ได้ |  |

<a id="schema-PaymentItemDetailDto"></a>
### `PaymentItemDetailDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `claimNo` | string | — | ได้ |  |
| `customerName` | string | — | ได้ |  |
| `transactionAmount` | number (double) | — | ได้ |  |
| `bankName` | string | — | ได้ |  |
| `approvedAmount` | number (double) | — | ได้ |  |

<a id="schema-PaymentStatusResponseDto"></a>
### `PaymentStatusResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

<a id="schema-PaymentTransactionResponseDto"></a>
### `PaymentTransactionResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentDate` | string (date-time) | — | ได้ |  |
| `printDate` | string (date-time) | — | ได้ |  |
| `admissionDate` | string (date-time) | — | ได้ |  |
| `countItem` | integer (int32) | — | — |  |
| `paymentId` | string (uuid) | — | — |  |
| `totalNetPaidAmount` | number (double) | — | ได้ |  |
| `customerName` | string | — | ได้ |  |
| `employee` | string | — | ได้ |  |
| `accountNo` | string | — | ได้ |  |
| `accountName` | string | — | ได้ |  |
| `bankName` | string | — | ได้ |  |
| `paymentCode` | string | — | ได้ |  |
| `transRefNo` | string | — | ได้ |  |
| `paymentItemDetail` | array&lt;[PaymentItemDetailDto](#schema-PaymentItemDetailDto)&gt; | — | ได้ |  |
| `paymentItemDetail[].claimNo` | string | — | ได้ |  |
| `paymentItemDetail[].customerName` | string | — | ได้ |  |
| `paymentItemDetail[].transactionAmount` | number (double) | — | ได้ |  |
| `paymentItemDetail[].bankName` | string | — | ได้ |  |
| `paymentItemDetail[].approvedAmount` | number (double) | — | ได้ |  |

<a id="schema-RemainingBalanceReponseDto"></a>
### `RemainingBalanceReponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

<a id="schema-RemainingBalanceRequestDto"></a>
### `RemainingBalanceRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `bankId` | integer (int32) | ✅ | — |  |
| `accountNo` | string | ✅ | — |  |
| `accountName` | string | ✅ | — |  |
| `balance` | number (double) | ✅ | — |  |

<a id="schema-SMSTransacttionSurveyRequest"></a>
### `SMSTransacttionSurveyRequest`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `smStransactionId` | string (uuid) | ✅ | — |  |
| `surveyId` | integer (int32) | ✅ | — |  |

<a id="schema-SMSTransacttionSurveyResponse"></a>
### `SMSTransacttionSurveyResponse`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `smStransactionId` | string (uuid) | — | ได้ |  |

<a id="schema-SaveSurveyFeedbackRequestDto"></a>
### `SaveSurveyFeedbackRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `surveyId` | integer (int32) | ✅ | — |  |
| `answersRequest` | [JToken](#schema-JToken) | ✅ | — |  |

<a id="schema-SaveSurveyFeedbackResponsetDto"></a>
### `SaveSurveyFeedbackResponsetDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `surveyId` | integer (int32) | — | — |  |
| `message` | string | — | ได้ |  |

<a id="schema-SearchClaimOrCaseResponseDto"></a>
### `SearchClaimOrCaseResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `caseId` | string (uuid) | — | — |  |
| `claimId` | string (uuid) | — | ได้ |  |
| `claimCase` | string | — | ได้ |  |
| `createdClaimDate` | string (date-time) | — | ได้ | วันที่สร้างเคลม |
| `customerName` | string | — | ได้ |  |
| `coverageType` | string | — | ได้ | ประเภทความคุ้มครอง |
| `caseAmount` | number (double) | — | ได้ | จำนวนเงิน (case) |
| `isClaimNo` | boolean | — | — | รายการที่ค้นหาเป็นเลขที่ ClaimNo หรือไม |

<a id="schema-SearchHospitalResponseDto"></a>
### `SearchHospitalResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `orgId` | integer (int32) | — | — |  |
| `orgName` | string | — | ได้ |  |

<a id="schema-SendEmailResponseDto"></a>
### `SendEmailResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

<a id="schema-SendEmailToHospitalRequestDto"></a>
### `SendEmailToHospitalRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentId` | string (uuid) | ✅ | — |  |

<a id="schema-TransferHospitalRequestDto"></a>
### `TransferHospitalRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paymentId` | string (uuid) | — | — | รหัสรายการจ่ายเงินโรงพยาบาลที่ต้องการโอน |

<a id="schema-TransferStatusResponseDto"></a>
### `TransferStatusResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `id` | integer (int32) | — | — |  |
| `name` | string | — | ได้ |  |

<a id="schema-UpdateHospitalSettingRequestDto"></a>
### `UpdateHospitalSettingRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `hospitalPaymentSettingId` | string (uuid) | — | — |  |
| `autoPayDelayDays` | integer (int32) | — | — |  |
| `holdStatusId` | integer (int32) | — | — |  |
| `isAutoPay` | boolean | — | — |  |

<a id="schema-UpdateHospitalSettingResponseDto"></a>
### `UpdateHospitalSettingResponseDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `isSuccess` | boolean | — | — |  |
| `message` | string | — | ได้ |  |

<a id="schema-UpdatePayTransferSettingRequestDto"></a>
### `UpdatePayTransferSettingRequestDto`

| ฟิลด์ | ชนิดข้อมูล | จำเป็น | Nullable | คำอธิบาย |
|---|---|:---:|:---:|---|
| `paytransferSettingId` | string (uuid) | ✅ | — |  |

[⬆ กลับสู่สารบัญ](#toc)

<a id="notes"></a>
## 7. ข้อสังเกตและข้อควรระวังจากการวิเคราะห์ Spec

ข้อสังเกตต่อไปนี้พบจากการอ่านไฟล์ OpenAPI ควรยืนยันกับทีมพัฒนาก่อนนำไปใช้งานจริง:

1. **`GET /api/RemainingBalance/InsertRemainingBalance` มี Request Body** — ปกติ HTTP GET ไม่ควรมี body และ client บางตัวไม่รองรับ อีกทั้งใช้ path เดียวกับ POST ที่เป็นการ Insert (ชื่อ Endpoint ไม่สื่อว่าเป็นการอ่านข้อมูล)
2. **`POST /api/Transfer/v1/CreateTransferResult`** — ชื่อใน spec ระบุว่า "Step 3 CreatedTransfer ทดสอบผลการสร้างรายการโอนเงิน" ดูเหมือนเป็น Endpoint สำหรับทดสอบ/รับผลจากระบบโอนเงิน และไม่ระบุ Response body
3. **`UpdateSettingAutoTransfer`** — Response `data` ใช้ชนิดเดียวกับ Request (`UpdatePayTransferSettingRequestDto`) แทนที่จะเป็น Response DTO แยก
4. **ชื่อฟิลด์สะกดผิดใน Response** — `CreateTransferResponseDTO.itemCout` (น่าจะเป็น `itemCount`) ต้องใช้ชื่อตามที่ API คืนจริง
5. **`GetHospitalTransferGroupStatuses`** ใช้ Response ชนิดเดียวกับ `GetPaymentStatuses` (`PaymentStatusResponseDto`)
6. **`GET /api/Transfer/v1/PaymentDetails`** ไม่มี `summary` ใน spec
7. **Endpoint ที่ไม่มี OAuth 2.0 security** — `RemainingBalance` (ใช้ Header `apiKey`) และ `Version/GetApplicationVersionControl` ส่วน 4 Endpoint ของกลุ่ม `Notification` มีทั้ง OAuth 2.0 และ Header `apiKey` (ดู Changelog) แต่ spec ไม่ระบุว่า `apiKey` บังคับหรือไม่
8. **ไม่มีการระบุ `servers`/Base URL** ใน spec — Base URL ของ UAT ในเอกสารนี้อนุมานจากที่อยู่ของไฟล์ swagger.json ส่วน Production ต้องขอจากผู้ดูแลระบบ
9. **Query Parameter ส่วนใหญ่ไม่ได้ระบุ `required`** แม้ชื่อจะสื่อว่าจำเป็น (เช่น `transferId`, `hospitalPaymentSettingId`) — ควรทดสอบพฤติกรรมจริง
10. **ฟิลด์ใน Request ที่ไม่มี `required`** ถือเป็น optional ตามนิยาม OpenAPI แต่ business rule อาจบังคับอยู่ที่ฝั่ง server

[⬆ กลับสู่สารบัญ](#toc)

<a id="changelog"></a>
## 8. บันทึกการเปลี่ยนแปลง (Changelog)

### 10 ตุลาคม 2569 — อัปเดตจากไฟล์ `ClaimfunAPISpecification.json`

เทียบกับฉบับก่อนหน้า (`ClaimFundAPI.json`): จำนวน Endpoint (32) และ Schema (72) เท่าเดิม ไม่มีรายการเพิ่มหรือลบ

| ประเภท | รายการ | การเปลี่ยนแปลง |
|---|---|---|
| Endpoint | [`GET /api/Notification/GetSurveyId`](#ep-get-api-notification-getsurveyid) | เพิ่ม Header parameter `apiKey` |
| Endpoint | [`GET /api/Notification/GetTransactionById`](#ep-get-api-notification-gettransactionbyid) | เพิ่ม Header parameter `apiKey` |
| Endpoint | [`POST /api/Notification/SaveSurveyFeedback`](#ep-post-api-notification-savesurveyfeedback) | เพิ่ม Header parameter `apiKey` |
| Endpoint | [`POST /api/Notification/UpdateSMSTransactionSurvey`](#ep-post-api-notification-updatesmstransactionsurvey) | เพิ่ม Header parameter `apiKey` |
| Schema | [`PaymentTransactionResponseDto`](#schema-PaymentTransactionResponseDto) | เพิ่มฟิลด์ `transRefNo` (string, nullable) |

> Endpoint ทั้ง 4 ยังคงกำหนด OAuth 2.0 (scope `ClaimFundAPI`) ไว้ด้วย จึงต้องส่งทั้ง Bearer token และ Header `apiKey` — ควรยืนยันกับทีมพัฒนาว่าต้องใช้ทั้งสองอย่างจริงหรือไม่

[⬆ กลับสู่สารบัญ](#toc)
