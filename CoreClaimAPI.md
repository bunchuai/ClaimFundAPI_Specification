# CoreClaim_api Specification

- **Title:** CoreClaim_api
- **Version:** v1
- **OpenAPI:** 3.0.1
- **Source:** `https://78nq1mrd-5001.asse.devtunnels.ms/swagger/v1/swagger.json`
- **รวม Endpoints:** 112 paths (112 operations)
- **รวม Schemas:** 309

> เอกสารนี้สร้างจากไฟล์ `swagger.json` ฉบับเต็มที่อัปโหลด ครอบคลุมทุก endpoint และทุก schema ที่มีอยู่ในไฟล์ต้นฉบับ

---

## สารบัญ

1. [ภาพรวม](#ภาพรวม)
2. [Authentication](#authentication)
3. [กลุ่ม ClaimFund](#กลุ่ม-claimfund) (38 endpoints)
   - [Masters](#claimfund-masters)
   - [Setting](#claimfund-setting)
   - [IncreaseTransfer](#claimfund-increasetransfer)
   - [AdditionalTransfer](#claimfund-additionaltransfer)
   - [Inquiry](#claimfund-inquiry)
   - [FailedPayTransfer](#claimfund-failedpaytransfer)
   - [Refund](#claimfund-refund)
4. [กลุ่ม CoreClaim](#กลุ่ม-coreclaim) (33 endpoints)
   - [customer](#coreclaim-customer)
   - [employee-payment-limit](#coreclaim-employee-payment-limit)
   - [calculate](#coreclaim-calculate)
   - [create](#coreclaim-create)
   - [document-subtype](#coreclaim-document-subtype)
   - [document](#coreclaim-document)
   - [claim](#coreclaim-claim)
   - [dashboard](#coreclaim-dashboard)
   - [policy](#coreclaim-policy)
   - [dcr](#coreclaim-dcr)
   - [standard-medical-expense](#coreclaim-standard-medical-expense)
5. [กลุ่ม Masters](#กลุ่ม-masters) (34 endpoints)
   - [users](#masters-users)
   - [zebracar](#masters-zebracar)
   - [title](#masters-title)
   - [relationtype](#masters-relationtype)
   - [beneficiary](#masters-beneficiary)
   - [simb](#masters-simb)
   - [benefit](#masters-benefit)
   - [province](#masters-province)
   - [branch](#masters-branch)
   - [bank](#masters-bank)
   - [hospital](#masters-hospital)
   - [school](#masters-school)
   - [insurance](#masters-insurance)
   - [disability](#masters-disability)
   - [paymentstatus](#masters-paymentstatus)
   - [adjustment](#masters-adjustment)
   - [deduction-source](#masters-deduction-source)
   - [claim](#masters-claim)
   - [document](#masters-document)
   - [bankaccount](#masters-bankaccount)
   - [contactperson](#masters-contactperson)
   - [noncoveredreason](#masters-noncoveredreason)
   - [incidenttype](#masters-incidenttype)
   - [formatType](#masters-formattype)
   - [chiefcomplaint](#masters-chiefcomplaint)
   - [icd10](#masters-icd10)
6. [กลุ่ม HospitalBilling](#กลุ่ม-hospitalbilling) (4 endpoints)
   - [billing](#hospitalbilling-billing)
7. [กลุ่ม IClaim](#กลุ่ม-iclaim) (3 endpoints)
   - [check-eligible](#iclaim-check-eligible)
   - [customer](#iclaim-customer)
   - [claim](#iclaim-claim)
8. [ภาคผนวก: Schemas ทั้งหมด](#ภาคผนวก-schemas-ทั้งหมด) (309 schemas)

---

## ภาพรวม

API นี้แบ่งเป็นกลุ่มตาม tag ดังนี้:

| กลุ่ม (Tag) | จำนวน Endpoints | คำอธิบาย |
|---|---|---|
| `ClaimFund` | 38 | จัดการการโอนเงิน/คืนเงินค่าสินไหมทดแทน (การตั้งค่า, โอนเพิ่ม, ขยายวงเงิน, สอบถามธนาคาร, การคืนเงิน) |
| `CoreClaim` | 33 | จัดการข้อมูลลูกค้า เคลม เอกสาร การคำนวณสินไหม และ dashboard ต่าง ๆ |
| `Masters` | 34 | ข้อมูล Master/รหัสอ้างอิงต่าง ๆ ที่ใช้ในระบบ (ผู้ใช้, จังหวัด, ธนาคาร, โรงพยาบาล, ICD-10 ฯลฯ) |
| `HospitalBilling` | 4 | จัดการรายการเรียกเก็บค่ารักษาพยาบาลจากโรงพยาบาล (Billing) |
| `IClaim` | 3 | ตรวจสอบสิทธิ์และข้อมูลเคลมสำหรับระบบ IClaim |

---

## Authentication

Endpoint ส่วนใหญ่ในกลุ่ม `ClaimFund` ระบุ security scheme เป็น **OAuth2** (`"security": [{ "OAuth2": [] }]`) และคืนค่า `401 Unauthorized` / `403 Forbidden` เป็นมาตรฐาน ส่วน endpoint ในกลุ่มอื่น (`CoreClaim`, `Masters`, `HospitalBilling`, `IClaim`) ไม่พบการระบุ security scheme อย่างชัดเจนในสเปกที่ให้มา

---

## กลุ่ม ClaimFund

### Masters

#### `GET /api/ClaimFund/Masters/GetAdjustmentReasonById`
**คำอธิบาย:** ข้อมูลสาเหตุการโอนเพิ่ม By Id  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `AdjustmentReasonId` | query | integer (int32) | No |  |

**Response (200):** [`AdjustmentReasonResponseDtoServiceResponse`](#schema-adjustmentreasonresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Masters/GetAdjustmentReasons`
**คำอธิบาย:** ข้อมูลสาเหตุการโอนเพิ่ม  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `AdjustmentTypeId` | query | integer (int32) | No |  |

**Response (200):** [`AdjustmentReasonResponseDtoListServiceResponse`](#schema-adjustmentreasonresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Masters/GetBankAccountRelationTypeById`
**คำอธิบาย:** ข้อมูลประเภทความสัมพันธ์ของบัญชี กับผู้รับสินไหม By Id  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `bankAccountRelationTypeId` | query | integer (int32) | No |  |

**Response (200):** [`BankAccountRelationTypeResponseDtoServiceResponse`](#schema-bankaccountrelationtyperesponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Masters/GetBankAccountRelationTypes`
**คำอธิบาย:** ข้อมูลประเภทความสัมพันธ์ของบัญชี กับผู้รับสินไหม  
**Security:** OAuth2  

**Response (200):** [`BankAccountRelationTypeResponseDtoListServiceResponse`](#schema-bankaccountrelationtyperesponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Masters/GetCaseRefundRejectReasons`
**คำอธิบาย:** สาเหตุที่ปฏิเสธ  
**Security:** OAuth2  

**Response (200):** [`CaseRefundRejectReasonResponseDtoListServiceResponse`](#schema-caserefundrejectreasonresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Masters/GetPaymentStatuses`
**คำอธิบาย:** ข้อมูลสถานะการจ่ายเงิน (Payment Status)  
**Security:** OAuth2  

**Response (200):** [`PaymentStatusResponseDtoListServiceResponse`](#schema-paymentstatusresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Masters/GetRefundReasonById`
**คำอธิบาย:** ข้อมูลสาเหตุการโอนคืน By Id  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `refundReasonId` | query | integer (int32) | No |  |

**Response (200):** [`RefundReasonResponseDtoServiceResponse`](#schema-refundreasonresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Masters/GetRefundReasons`
**คำอธิบาย:** ข้อมูลสาเหตุการโอนคืน  
**Security:** OAuth2  

**Response (200):** [`RefundReasonResponseDtoListServiceResponse`](#schema-refundreasonresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Masters/GetRefundStatus`
**คำอธิบาย:** Get สถานะการคืนเงิน  
**Security:** OAuth2  

**Response (200):** [`RefundStatusResponseDtoListServiceResponse`](#schema-refundstatusresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

### Setting

#### `GET /api/ClaimFund/Setting/GetCurrentSetting`
**คำอธิบาย:** ตั้งค่าการโอนเงิน  
**Security:** OAuth2  

**Response (200):** [`PayTransferSettingResponseDtoServiceResponse`](#schema-paytransfersettingresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Setting/SearchClaimOrCase`
**คำอธิบาย:** ค้นหารายการจาก เลขที่ ClaimNo / CaseNo  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchDetail` | query | string | Yes |  |

**Response (200):** [`SearchClaimOrCaseResponseDtoListServiceResponse`](#schema-searchclaimorcaseresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/Setting/UpdateSettingAutoTransfer`
**คำอธิบาย:** บันทึกการตั้งค่าการโอนเงิน  
**Security:** OAuth2  

**Request Body:** [`UpdatePayTransferSettingRequestDto`](#schema-updatepaytransfersettingrequestdto)

**Response (200):** [`UpdatePayTransferSettingRequestDtoServiceResponse`](#schema-updatepaytransfersettingrequestdtoserviceresponse)

**Other status codes:** 401, 403

---

### IncreaseTransfer

#### `GET /api/ClaimFund/IncreaseTransfer/GetIncreaseTransferLimitDetail`
**คำอธิบาย:** รายละเอียด ขยายวงเงิน  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseTransferApprovalId` | query | string (uuid) | No |  |

**Response (200):** [`GetIncreaseTransferLimitDetailResponseDtoServiceResponse`](#schema-getincreasetransferlimitdetailresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/IncreaseTransfer/IncreaseTransferLimitChangeStatus`
**คำอธิบาย:** บันทึกรายการ ขยายวงเงิน  
**Security:** OAuth2  

**Request Body:** [`IncreaseTransferLimitChangeStatusRequestDto`](#schema-increasetransferlimitchangestatusrequestdto)

**Response (200):** [`IncreaseTransferLimitChangeStatusResponseDtoServiceResponse`](#schema-increasetransferlimitchangestatusresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/IncreaseTransfer/IncreaseTransferLimitMonitors`
**คำอธิบาย:** Monitor ขยายวงเงิน  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`IncreaseTransferLimitMonitorResponseDtoListServiceResponse`](#schema-increasetransferlimitmonitorresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

### AdditionalTransfer

#### `GET /api/ClaimFund/AdditionalTransfer/AdditionalTransferDetails`
**คำอธิบาย:** โอนเพิ่ม รายละเอียด tep(1)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |

**Response (200):** [`AdditionalTransferDetailsResponseDtoServiceResponse`](#schema-additionaltransferdetailsresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/AdditionalTransfer/AdditionalTransferMonitor`
**คำอธิบาย:** ตรวจสอบรายการโอนเงินเพิ่ม (Monitor)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Request Body:** [`AdditionalTransferMonitorRequestDto`](#schema-additionaltransfermonitorrequestdto)

**Response (200):** [`usp_AdditionalTransferMonitor_SelectResultListServiceResponse`](#schema-usp_additionaltransfermonitor_selectresultlistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/AdditionalTransfer/GetAdditionalTransferAccountDetail`
**คำอธิบาย:** รายละเอียดบัญชี (แก้ไขการโอนเงิน)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `paymentId` | query | string (uuid) | No |  |

**Response (200):** [`AdditionalTransferAccountDetailsResponseDtoListServiceResponse`](#schema-additionaltransferaccountdetailsresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/AdditionalTransfer/GetClaimTransaction`
**คำอธิบาย:** ดึงประวัติการทำรายการ (Transaction) ของเคลม  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`AdditionalTransferTransactionResponseDtoListServiceResponse`](#schema-additionaltransfertransactionresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/AdditionalTransfer/GetClaimTransactions`
**คำอธิบาย:** ประวัติการทำรายการ  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`AdditionalTransferTransactionResponseDtoListServiceResponse`](#schema-additionaltransfertransactionresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/AdditionalTransfer/GetDecreaseTransaction`
**คำอธิบาย:** ประวัติการลดยอด (โอนเพิ่ม)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetDecreaseTransactionResponseDtoListServiceResponse`](#schema-getdecreasetransactionresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/AdditionalTransfer/SaveAdditionalTransfer`
**คำอธิบาย:** บันทึกการโอนเงินเพิ่ม  
**Security:** OAuth2  

**Request Body:** [`SaveAdditionalTransferRequest`](#schema-saveadditionaltransferrequest)

**Response (200):** [`SaveAdditionalTransferResponseDtoServiceResponse`](#schema-saveadditionaltransferresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/AdditionalTransfer/TransferHistory`
**คำอธิบาย:** ประวัติการโอนเงิน  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`TransferTransactionResponseDtoServiceResponse`](#schema-transfertransactionresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/AdditionalTransfer/UpdateAdditionalTransfer`
**คำอธิบาย:** แก้ไขการโอนเงินเพิ่ม  
**Security:** OAuth2  

**Request Body:** [`UpdateAdditionalTransferRequestDto`](#schema-updateadditionaltransferrequestdto)

**Response (200):** [`UpdateAdditionalTransferResponseDtoServiceResponse`](#schema-updateadditionaltransferresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

### Inquiry

#### `GET /api/ClaimFund/Inquiry/InquiryDetail`
**คำอธิบาย:** สอบถามธนาคาร Inquiry Monitor รายละเอียด  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `PayTransferTransactionId` | query | string (uuid) | Yes |  |

**Response (200):** [`BankInquiryDetailResponseDtoServiceResponse`](#schema-bankinquirydetailresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Inquiry/InquiryMonitors`
**คำอธิบาย:** สอบถามธนาคาร Inquiry Monitor  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`usp_InquiryMonitor_SelectResultListServiceResponse`](#schema-usp_inquirymonitor_selectresultlistserviceresponse)

**Other status codes:** 401, 403

---

### FailedPayTransfer

#### `GET /api/ClaimFund/FailedPayTransfer/FailedPayTransferTransactionDetail`
**คำอธิบาย:** รายละเอียด Monitor - แก้ไขการโอนเงิน  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `PayTransferTransactionId` | query | string (uuid) | Yes |  |

**Response (200):** [`FailedPayTransferTransactionResponseDtoServiceResponse`](#schema-failedpaytransfertransactionresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/FailedPayTransfer/FailedPayTransferTransactionMonitor`
**คำอธิบาย:** Monitor - แก้ไขการโอนเงิน  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`usp_FailedPayTransferTransaction_SelectResultListServiceResponse`](#schema-usp_failedpaytransfertransaction_selectresultlistserviceresponse)

**Other status codes:** 401, 403

---

### Refund

#### `GET /api/ClaimFund/Refund/CaseRefundApproveDetail`
**คำอธิบาย:** รายละเอียดการ อนุมัติคืนเงิน  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseRefundId` | query | string (uuid) | No |  |

**Response (200):** [`CaseRefundApproveDetailResponseDtoServiceResponse`](#schema-caserefundapprovedetailresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/Refund/CaseRefundApproveUpdateStatus`
**คำอธิบาย:** อนุมัติคืนเงิน /ปฎิเสษ  
**Security:** OAuth2  

**Request Body:** [`CaseRefundApproveUpdateStatusRequestDto`](#schema-caserefundapproveupdatestatusrequestdto)

**Response (200):** [`CaseRefundApproveUpdateStatusResponseDtoServiceResponse`](#schema-caserefundapproveupdatestatusresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/Refund/CreateCaseRefund`
**คำอธิบาย:** สร้างรายการเงินคืนเงิน  
**Security:** OAuth2  

**Request Body:** [`CreateRefundRequestDto`](#schema-createrefundrequestdto)

**Response (200):** [`CreateRefundResponsetDtoServiceResponse`](#schema-createrefundresponsetdtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Refund/GetClaimTransaction`
**คำอธิบาย:** ดึงประวัติการทำรายการ (Transaction) ของเคลมสำหรับการคืนเงิน (Refund)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`RefundTransactionResponseDtoListServiceResponse`](#schema-refundtransactionresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Refund/GetDecreaseTransaction`
**คำอธิบาย:** ประวัติการลดยอดสำหรับการคืนเงิน (Refund)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetDecreaseTransactionRefundResponseDtoListServiceResponse`](#schema-getdecreasetransactionrefundresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/Refund/RefundApproveMonitor`
**คำอธิบาย:** Monitor approve  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Request Body:** [`RefundApproveMonitorRequestDto`](#schema-refundapprovemonitorrequestdto)

**Response (200):** [`RefundApproveMonitorResponseListServiceResponse`](#schema-refundapprovemonitorresponselistserviceresponse)

**Other status codes:** 401, 403

---

#### `POST /api/ClaimFund/Refund/RefundMonitor`
**คำอธิบาย:** Monitor การคืนเงิน (Refund)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Request Body:** [`RefundMonitorRequestDto`](#schema-refundmonitorrequestdto)

**Response (200):** [`RefundMonitorResponseListServiceResponse`](#schema-refundmonitorresponselistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Refund/SaveRefundAccountDetail`
**คำอธิบาย:** รายละเอียดบัญชีสำหรับการคืนเงิน (Refund)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `paymentId` | query | string (uuid) | Yes |  |

**Response (200):** [`RefundDetailsAccountDetailsResponseDtoListServiceResponse`](#schema-refunddetailsaccountdetailsresponsedtolistserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Refund/SaveRefundDetails`
**คำอธิบาย:** รายละเอียดการคืนเงิน (Refund)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | Yes |  |

**Response (200):** [`SaveRefundDetailsResponseDtoServiceResponse`](#schema-saverefunddetailsresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

#### `GET /api/ClaimFund/Refund/TransferHistory`
**คำอธิบาย:** ประวัติการโอนเงินสำหรับการคืนเงิน (Refund)  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`RefundTransferTransactionResponseDtoServiceResponse`](#schema-refundtransfertransactionresponsedtoserviceresponse)

**Other status codes:** 401, 403

---

## กลุ่ม CoreClaim

### customer

#### `GET /api/customer/benefit-detail/half`
**คำอธิบาย:** API สำหรับ Get Customer Benefit Detail Half  
**operationId:** `GetCustomerBenefitDetailHalf`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `policyCode` | query | string | No |  |
| `incidentDate` | query | string (date-time) | No |  |
| `isContinue` | query | boolean | No |  |
| `incidentTypeId` | query | integer (int32) | No |  |
| `coverageTypeId` | query | integer (int32) | No |  |
| `medicalTypeId` | query | integer (int32) | No |  |
| `causeOfIncidentId` | query | integer (int32) | No |  |
| `formatTypeId` | query | integer (int32) | No |  |
| `CusTomerTypeCode` | query | string | No |  |
| `CustomerDetailId` | query | string (uuid) | No |  |
| `claimNo` | query | string | No |  |

**Response (200):** [`GetCustomerBenefitDetailHalfDtoResponseListServiceResponse`](#schema-getcustomerbenefitdetailhalfdtoresponselistserviceresponse)

---

#### `GET /api/customer/benefit-detail/search`
**คำอธิบาย:** API สำหรับ Search Customer Benefit Detail  
**operationId:** `GetCustomerBenefitDetailSearch`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `policyCode` | query | string | No |  |
| `incidentDate` | query | string (date-time) | No |  |
| `isContinue` | query | boolean | No |  |
| `incidentTypeId` | query | integer (int32) | No |  |
| `coverageTypeId` | query | integer (int32) | No |  |
| `medicalTypeId` | query | integer (int32) | No |  |
| `claimNo` | query | string | No |  |
| `customerTypeCode` | query | string | No |  |

**Response (200):** [`GetCustomerBenefitDetailSearchDtoResponseListServiceResponse`](#schema-getcustomerbenefitdetailsearchdtoresponselistserviceresponse)

---

#### `GET /api/customer/contact-person`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Contact Person  
**operationId:** `GetContactPerson`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `applicationId` | query | string | Yes |  |
| `productTypeId` | query | integer (int32) | No |  |

**Response (200):** [`GetContactPersonDtoResponseListServiceResponse`](#schema-getcontactpersondtoresponselistserviceresponse)

---

#### `GET /api/customer/policybenefit-shered`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Policy Benefit Shered (สิทธิประโยชน์ร่วม)  
**operationId:** `GetPolicyBenefitShered`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `policyCode` | query | string | No |  |
| `customerTypeCode` | query | string | No |  |

**Response (200):** [`GetPolicyBenefitSheredDtoResponseListServiceResponse`](#schema-getpolicybenefitshereddtoresponselistserviceresponse)

---

#### `GET /api/customer/search`
**คำอธิบาย:** API สำหรับ Search Customer  
**operationId:** `GetCustomerSearch`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchIndex` | query | integer (int32) | No |  |
| `isSeachDetail` | query | boolean | No |  |
| `incidentDate` | query | string (date-time) | No |  |
| `schoolId` | query | integer (int32) | No |  |
| `provinceId` | query | integer (int32) | No |  |
| `incidentTypeId` | query | integer (int32) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetCustomerSearchDtoResponseListServiceResponse`](#schema-getcustomersearchdtoresponselistserviceresponse)

---

#### `GET /api/customer/search-by-policy-code`
**คำอธิบาย:** API สำหรับ Search Customer By PolicyCode  
**operationId:** `GetCustomerSearchByPolicyCode`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `policyCode` | query | string | No |  |
| `searchIndex` | query | integer (int32) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetCustomerSearchByPolicyCodeDtoResponseListServiceResponse`](#schema-getcustomersearchbypolicycodedtoresponselistserviceresponse)

---

#### `GET /api/customer/{customerDetailId}/detail`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Customer Detail By Id  
**operationId:** `GetCustomerDetailById`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `customerDetailId` | path | string (uuid) | Yes |  |

**Response (200):** [`GetCustomerDetailByIdDtoResponseServiceResponse`](#schema-getcustomerdetailbyiddtoresponseserviceresponse)

---

#### `GET /api/customer/{policyCode}/bank-account`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Customer BankAccount  
**operationId:** `GetCustomerBankAccount`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `policyCode` | path | string | Yes |  |

**Response (200):** [`GetCustomerBankAccountDtoResponseListServiceResponse`](#schema-getcustomerbankaccountdtoresponselistserviceresponse)

---

### employee-payment-limit

#### `GET /api/employee-payment-limit/{userId}`
**คำอธิบาย:** ตรวจวงเงินรายวันก่อนบันทึก Case โดยใช้วันที่จาก request หากระบุ มิฉะนั้นใช้วันที่ปัจจุบันของ Server และไม่จองวงเงิน  
**operationId:** `GetEmployeeClaimPaymentLimit`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `userId` | path | integer (int32) | Yes |  |
| `RequestedTransferAmount` | query | number (double) | Yes |  |
| `RequestedDate` | query | string (date-time) | No |  |

**Response (200):** [`GetEmployeeClaimPaymentLimitResponseServiceResponse`](#schema-getemployeeclaimpaymentlimitresponseserviceresponse)

---

### calculate

#### `POST /api/calculate/caseclaim`
**คำอธิบาย:** API สำหรับ Calculate ข้อมูล Case Claim  
**operationId:** `CalculateCaseClaim`  

**Request Body:** [`CalculateCaseClaimDtoRequest`](#schema-calculatecaseclaimdtorequest)

**Response (200):** [`CalculateCaseClaimDtoResponseServiceResponse`](#schema-calculatecaseclaimdtoresponseserviceresponse)

---

#### `GET /api/calculate/disability`
**คำอธิบาย:** API สำหรับ Calculate ข้อมูล Case Disability (สูยเสียอวัยวะ)  
**operationId:** `CalculateCaseDisability`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `customerDetailId` | query | string (uuid) | No |  |
| `bodyPartId` | query | integer (int32) | No |  |
| `standardMedicalExpenseId` | query | integer (int32) | No |  |

**Response (200):** [`CalculateCaseDisabilityDtoResponseServiceResponse`](#schema-calculatecasedisabilitydtoresponseserviceresponse)

---

### create

#### `POST /api/create/case-adjudication`
**คำอธิบาย:** สร้างผลการพิจารณาเริ่มต้นของ Case โดยอ้างอิง CaseId  
**operationId:** `CreateCaseAdjudication`  

**Request Body:** [`CreateCaseAdjudicationDtoRequest`](#schema-createcaseadjudicationdtorequest)

**Response (200):** [`CreateCaseAdjudicationDtoResponseServiceResponse`](#schema-createcaseadjudicationdtoresponseserviceresponse)

---

#### `POST /api/create/continued-claim`
**คำอธิบาย:** เพิ่ม Case ใหม่ภายใต้ Claim เดิมโดยอ้างอิง ClaimId  
**operationId:** `CreateContinuedClaim`  

**Request Body:** [`CreateContinuedClaimDtoRequest`](#schema-createcontinuedclaimdtorequest)

**Response (200):** [`CreateCoreClaimDtoResponseServiceResponse`](#schema-createcoreclaimdtoresponseserviceresponse)

---

#### `POST /api/create/coreclaim`
**คำอธิบาย:** API สำหรับ Create ข้อมูล CoreClaim  
**operationId:** `CreateCoreClaim`  

**Request Body:** [`CreateCoreClaimV2DtoRequest`](#schema-createcoreclaimv2dtorequest)

**Response (200):** [`CreateCoreClaimDtoResponseServiceResponse`](#schema-createcoreclaimdtoresponseserviceresponse)

---

### document-subtype

#### `POST /api/document-subtype`
**คำอธิบาย:** API สำหรับ Get ข้อมูล DocumentSubType , documentTypeId : เอกสารของโปรเจค , documentPrefix : คำนำหน้ารหัสเอกสาร, documentSubTypeIdList = รายการรหัสเอกสารย่อยที่ต้องการแสดง  
**operationId:** `GetDocumentSubType`  

**Request Body:** [`GetDocumentSubTypeDtoRequest`](#schema-getdocumentsubtypedtorequest)

**Response (200):** [`GetDocumentSubTypeDtoResponseListServiceResponse`](#schema-getdocumentsubtypedtoresponselistserviceresponse)

---

### document

#### `GET /api/document/case/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Document By CaseId  
**operationId:** `GetDocumentByCaseId`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | No |  |
| `productTypeId` | query | integer (int32) | No |  |
| `claimSourceId` | query | integer (int32) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetDocumentByCaseIdDtoResponseListServiceResponse`](#schema-getdocumentbycaseiddtoresponselistserviceresponse)

---

#### `GET /api/document/case/{caseId}/overview`
**คำอธิบาย:** API สำหรับแสดงข้อมูลภาพรวมการตรวจสอบเอกสาร ผลการพิจารณา และรายการค่าใช้จ่ายของ Case  
**operationId:** `GetCaseReviewOverview`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | path | string (uuid) | Yes |  |

**Response (200):** [`GetCaseReviewOverviewDtoResponseServiceResponse`](#schema-getcasereviewoverviewdtoresponseserviceresponse)

---

### claim

#### `GET /api/claim/case/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Case By ClaimId  
**operationId:** `GetCaseByClaimId`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `claimId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetCaseByClaimIdDtoResponseListServiceResponse`](#schema-getcasebyclaimiddtoresponselistserviceresponse)

---

#### `GET /api/claim/continue/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล การเคลมต่อเนื่อง  
**operationId:** `GetClaimContinue`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `applicationId` | query | string | No |  |
| `initialCaseId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetClaimContinueDtoResponseListServiceResponse`](#schema-getclaimcontinuedtoresponselistserviceresponse)

---

#### `GET /api/claim/customer-monitor/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล CustomerClaim Adjudication Monitor  
**operationId:** `GetCustomerClaimAdjudicationMonitor`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `dateOption` | query | integer (int32) | No |  |
| `dateFrom` | query | string (date-time) | No |  |
| `dateTo` | query | string (date-time) | No |  |
| `isProductTypeId_PH` | query | boolean | No |  |
| `isProductTypeId_PA` | query | boolean | No |  |
| `claimTransactionTypeId` | query | integer (int32) | No |  |
| `searchOption` | query | integer (int32) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetCustomerClaimAdjudicationMonitorDtoResponseListServiceResponse`](#schema-getcustomerclaimadjudicationmonitordtoresponselistserviceresponse)

---

#### `POST /api/claim/decision`
**คำอธิบาย:** API สำหรับ บันทึกผลพิจารณาเคลม  
**operationId:** `UpsertClaimDecision`  

**Request Body:** [`UpsertClaimDecisionDtoRequest`](#schema-upsertclaimdecisiondtorequest)

**Response (200):** [`UpsertClaimDecisionDtoResponseServiceResponse`](#schema-upsertclaimdecisiondtoresponseserviceresponse)

---

#### `POST /api/claim/decision/approve`
**คำอธิบาย:** อนุมัติผลพิจารณา  
**operationId:** `ApproveClaimDecision`  

**Request Body:** [`ApproveClaimDecisionDtoRequest`](#schema-approveclaimdecisiondtorequest)

**Response (200):** [`UpsertClaimDecisionDtoResponseServiceResponse`](#schema-upsertclaimdecisiondtoresponseserviceresponse)

---

#### `POST /api/claim/decision/draft`
**คำอธิบาย:** API สำหรับ บันทึกผลพิจารณาเคลม [บันทึกแบบร่าง]  
**operationId:** `SaveClaimEditDraft`  

**Request Body:** [`SaveClaimEditDraftDtoRequest`](#schema-saveclaimeditdraftdtorequest)

**Response (200):** [`SaveClaimEditDraftDtoResponeServiceResponse`](#schema-saveclaimeditdraftdtoresponeserviceresponse)

---

#### `GET /api/claim/decision/draft/revision`
**คำอธิบาย:** API สำหรับอ่าน Claim Edit Draft Revision พร้อมข้อมูล Payload ที่แปลงแล้ว  
**operationId:** `GetClaimEditDraftRevision`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `DraftRevisionId` | query | string (uuid) | No |  |

**Response (200):** [`GetClaimEditDraftRevisionDtoResponseServiceResponse`](#schema-getclaimeditdraftrevisiondtoresponseserviceresponse)

**Other status codes:** 500

---

#### `GET /api/claim/detail/consider/{claimId}/{caseId}`
**คำอธิบาย:** API สำหรับ Get ข้อมูลรายละเอียด Claim พิจารณา  
**operationId:** `GetClaimDetailConsider`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `claimId` | path | string (uuid) | Yes |  |
| `caseId` | path | string (uuid) | Yes |  |

**Response (200):** [`GetClaimDetailConsiderDtoResponseServiceResponse`](#schema-getclaimdetailconsiderdtoresponseserviceresponse)

---

#### `GET /api/claim/history/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล ประวัติการเคลม  
**operationId:** `GetClaimHistory`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `applicationId` | query | string | No |  |
| `incidentTypeId` | query | integer (int32) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetClaimHistoryDtoResponseListServiceResponse`](#schema-getclaimhistorydtoresponselistserviceresponse)

---

#### `GET /api/claim/hospital-monitor/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล HospitalClaim Adjudication Monitor  
**operationId:** `GetHospitalClaimAdjudicationMonitor`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `dateOption` | query | integer (int32) | No |  |
| `dateFrom` | query | string (date-time) | No |  |
| `dateTo` | query | string (date-time) | No |  |
| `isProductTypeId_PH` | query | boolean | No |  |
| `isProductTypeId_PA` | query | boolean | No |  |
| `claimTransactionTypeId` | query | integer (int32) | No |  |
| `searchOption` | query | integer (int32) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetHospitalClaimAdjudicationMonitorDtoResponseListServiceResponse`](#schema-gethospitalclaimadjudicationmonitordtoresponselistserviceresponse)

---

#### `GET /api/claim/transaction-log/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล TransactionLog Claim (ประวัติการทำรายการ)  
**operationId:** `GetClaimTransactionLog`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `claimId` | query | string (uuid) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetClaimTransactionLogDtoResponseListServiceResponse`](#schema-getclaimtransactionlogdtoresponselistserviceresponse)

---

#### `GET /api/claim/{claimId}/previous`
**คำอธิบาย:** API สำหรับ Get ข้อมูลเคลมก่อนหน้า  
**operationId:** `GetPreviousClaim`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `claimId` | path | string (uuid) | Yes |  |

**Response (200):** [`GetPreviousClaimDtoResponseServiceResponse`](#schema-getpreviousclaimdtoresponseserviceresponse)

---

### dashboard

#### `GET /api/dashboard/customer-consider/filter`
**คำอธิบาย:** Service สำหรับ Get ข้อมูล Dashboard Customer Consider (พิจารณาเคลมลูกค้า)  
**operationId:** `GetDashboardCustomerConsider`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `dateOption` | query | integer (int32) | No |  |
| `dateFrom` | query | string (date-time) | No |  |
| `dateTo` | query | string (date-time) | No |  |

**Response (200):** [`GetDashboardCustomerConsiderDtoResponseListServiceResponse`](#schema-getdashboardcustomerconsiderdtoresponselistserviceresponse)

---

### policy

#### `GET /api/policy/benefit`
**คำอธิบาย:** API สำหรับ Get ข้อมูล PolicyBenefit (ความคุ้มครอง)  
**operationId:** `GetPolicyBenefit`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `productTypeId` | query | integer (int32) | No |  |
| `applicationCode` | query | string | No |  |
| `productId` | query | integer (int32) | No |  |
| `customerTypeCode` | query | string | No |  |

**Response (200):** [`GetPolicyBenefitDtoResponseListServiceResponse`](#schema-getpolicybenefitdtoresponselistserviceresponse)

---

### dcr

#### `GET /api/dcr/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล DCR (การชำระเงิน)  
**operationId:** `GetDCR`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `applicationCode` | query | string | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetDCRDtoResponseListServiceResponse`](#schema-getdcrdtoresponselistserviceresponse)

---

### standard-medical-expense

#### `GET /api/standard-medical-expense/case`
**คำอธิบาย:** API สำหรับ Get ข้อมูล รายการค่ารักษา(เบื้องต้น) Default จาก CaseItem จากหน้าแจ้งเคลม  
**operationId:** `GetStandardMedicalExpenseByCase`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `caseId` | query | string (uuid) | Yes |  |
| `formatTypeId` | query | integer (int32) | No |  |
| `coverageTypeId` | query | integer (int32) | No |  |
| `medicalTypeId` | query | integer (int32) | No |  |
| `productTypeId` | query | integer (int32) | Yes |  |
| `causeOfIncidentId` | query | integer (int32) | No |  |
| `productId` | query | integer (int32) | No |  |
| `applicationCode` | query | string | No |  |
| `customerTypeCode` | query | string | No |  |

**Response (200):** [`GetStandardMedicalExpenseByCaseDtoResponseListServiceResponse`](#schema-getstandardmedicalexpensebycasedtoresponselistserviceresponse)

---

## กลุ่ม Masters

### users

#### `GET /api/Masters/users`
**คำอธิบาย:** API สำหรับ Get ข้อมูลพนักงาน  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `userId` | query | integer (int32) | No |  |

**Response (200):** [`AllUserDtoResponseListServiceResponse`](#schema-alluserdtoresponselistserviceresponse)

---

### zebracar

#### `GET /api/Masters/zebracar/owner`
**คำอธิบาย:** API สำหรับ Get ข้อมูล ZebraCarOwner (เจ้าของรถม้าลาย)  
**operationId:** `GetZebraCarOwner`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `zebraId` | query | integer (int32) | No |  |
| `employeeId` | query | integer (int32) | No |  |

**Response (200):** [`GetZebraCarOwnerDtoResponseListServiceResponse`](#schema-getzebracarownerdtoresponselistserviceresponse)

---

### title

#### `GET /api/Masters/title`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Title (คำนำหน้าชื่อ) personTypeId : 1, 3 = สถานที่ , 2 = บุคคล  
**operationId:** `GetTitle`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `titleId` | query | integer (int32) | No |  |
| `personTypeId` | query | integer (int32) | No |  |

**Response (200):** [`GetTitleDtoResponseListServiceResponse`](#schema-gettitledtoresponselistserviceresponse)

---

### relationtype

#### `GET /api/Masters/relationtype`
**คำอธิบาย:** API สำหรับ Get ข้อมูล RelationType (ประเภทความสัมพันธ์)  
**operationId:** `GetRelationType`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `relationTypeId` | query | integer (int32) | No |  |

**Response (200):** [`GetRelationTypeDtoResponseListServiceResponse`](#schema-getrelationtypedtoresponselistserviceresponse)

---

### beneficiary

#### `GET /api/Masters/beneficiary`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Beneficiary (ผู้รับผลประโยชน์) By PolicyCode  
**operationId:** `GetBeneficiary`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `policyCode` | query | string | No |  |

**Response (200):** [`GetBeneficiaryDtoResponseListServiceResponse`](#schema-getbeneficiarydtoresponselistserviceresponse)

---

### simb

#### `GET /api/Masters/simb`
**คำอธิบาย:** API สำหรับ Get ข้อมูล SIMB  
**operationId:** `GetSimB`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `formatTypeId` | query | integer (int32) | No |  |
| `coverageTypeId` | query | integer (int32) | No |  |
| `medicalTypeId` | query | integer (int32) | No |  |
| `isUseOften` | query | boolean | No |  |
| `productTypeId` | query | integer (int32) | No |  |
| `causeOfIncidentId` | query | integer (int32) | No |  |
| `plandId` | query | integer (int32) | No |  |

**Response (200):** [`InputToStandardMappingDtoResponseListServiceResponse`](#schema-inputtostandardmappingdtoresponselistserviceresponse)

---

#### `GET /api/Masters/simb/category`
**คำอธิบาย:** API สำหรับ Get ข้อมูล SIMB Category  
**operationId:** `GetSimBCategory`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `formatTypeId` | query | integer (int32) | No |  |
| `coverageTypeId` | query | integer (int32) | No |  |
| `medicalTypeId` | query | integer (int32) | No |  |
| `productTypeId` | query | integer (int32) | No |  |
| `causeOfIncidentId` | query | integer (int32) | No |  |
| `planId` | query | integer (int32) | No |  |

**Response (200):** [`StandardMedicalExpenseCategoryDtoResponseListServiceResponse`](#schema-standardmedicalexpensecategorydtoresponselistserviceresponse)

---

### benefit

#### `GET /api/Masters/benefit`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Benefit (ผลประโยชน์)  
**operationId:** `GetBenefit`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `benefitId` | query | integer (int32) | No |  |
| `benefitIdList` | query | array of integer (int32) | No |  |

**Response (200):** [`GetBenefitDtoResponseListServiceResponse`](#schema-getbenefitdtoresponselistserviceresponse)

---

### province

#### `GET /api/Masters/province`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Province List (จังหวัด)  
**operationId:** `GetProvince`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `provinceId` | query | integer (int32) | No |  |

**Response (200):** [`GetProvinceDtoResponseListServiceResponse`](#schema-getprovincedtoresponselistserviceresponse)

---

### branch

#### `GET /api/Masters/branch`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Branch (สาขา)  
**operationId:** `GetBranch`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `branchId` | query | integer (int32) | No |  |

**Response (200):** [`GetBranchDtoResponseListServiceResponse`](#schema-getbranchdtoresponselistserviceresponse)

---

### bank

#### `GET /api/Masters/bank`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Organize (ธนาคาร)  
**operationId:** `GetAllBank`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `organizeId` | query | integer (int32) | No |  |

**Response (200):** [`GetOrganizeDtoResponseListServiceResponse`](#schema-getorganizedtoresponselistserviceresponse)

---

### hospital

#### `GET /api/Masters/hospital`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Organize (โรงพยาบาล)  
**operationId:** `GetAllHospital`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `organizeId` | query | integer (int32) | No |  |

**Response (200):** [`GetOrganizeDtoResponseListServiceResponse`](#schema-getorganizedtoresponselistserviceresponse)

---

### school

#### `GET /api/Masters/school`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Organize (โรงเรียน)  
**operationId:** `GetSchoolByProvinceId`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `provinceId` | query | integer (int32) | No |  |
| `organizeId` | query | integer (int32) | No |  |

**Response (200):** [`GetOrganizeDtoResponseListServiceResponse`](#schema-getorganizedtoresponselistserviceresponse)

---

### insurance

#### `GET /api/Masters/insurance/filter`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Organize (บริษัทประกัน)  
**operationId:** `GetInsuranceCompany`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `organizeId` | query | integer (int32) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`GetInsuranceCompanyDtoResponseListServiceResponse`](#schema-getinsurancecompanydtoresponselistserviceresponse)

---

### disability

#### `GET /api/Masters/disability/bodypart`
**คำอธิบาย:** API สำหรับ Get จ้อมูล BodyPart by DisabilityLossPartId  
**operationId:** `GetBodyPartByDisabilityLossPart`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `disabilityLossPartId` | query | integer (int32) | No |  |

**Response (200):** [`GetBodyPartByDisabilityLossPartDtoResponseListServiceResponse`](#schema-getbodypartbydisabilitylosspartdtoresponselistserviceresponse)

---

#### `GET /api/Masters/disability/losspart`
**คำอธิบาย:** API สำหรับ Get ข้อมูล DisabilityLossPart (ส่วนของร่างกายที่พิการ)  
**operationId:** `GetDisabilityLossPart`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `disabilityLossPartId` | query | integer (int32) | No |  |

**Response (200):** [`GetDisabilityLossPartDtoResponseListServiceResponse`](#schema-getdisabilitylosspartdtoresponselistserviceresponse)

---

### paymentstatus

#### `GET /api/Masters/paymentstatus`
**คำอธิบาย:** API สำหรับ Get ข้อมูล PaymentStatus (สถานะการโอนเงิน)  
**operationId:** `GetPaymentStatus`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `paymentStatusId` | query | integer (int32) | No |  |

**Response (200):** [`GetPaymentStatusDtoResponseListServiceResponse`](#schema-getpaymentstatusdtoresponselistserviceresponse)

---

### adjustment

#### `GET /api/Masters/adjustment/reason`
**คำอธิบาย:** API สำหรับ Get ข้อมูล AdjustmentReason (AdjustmentTypeId 2 = โอนเพิ่ม, 3 = คืนเงิน)  
**operationId:** `GetAdjustmentReason`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `adjustmentTypeId` | query | integer (int32) | No |  |
| `adjustmentReasonId` | query | integer (int32) | No |  |

**Response (200):** [`GetAdjustmentReasonDtoResponseListServiceResponse`](#schema-getadjustmentreasondtoresponselistserviceresponse)

---

### deduction-source

#### `GET /api/Masters/deduction-source`
**คำอธิบาย:** API สำหรับ Get ข้อมูล DeductionSource (ช่องทางการหักเงิน)  
**operationId:** `GetDeductionSource`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `deductionSourceId` | query | integer (int32) | No |  |

**Response (200):** [`GetDeductionSourceDtoResponseListServiceResponse`](#schema-getdeductionsourcedtoresponselistserviceresponse)

---

### claim

#### `GET /api/Masters/claim/cancel/reason`
**คำอธิบาย:** API สำหรับ Get ข้อมูล CancelReason (เหตุผลการยกเลิก)  
**operationId:** `GetCancelReason`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `cancelReasonId` | query | integer (int32) | No |  |

**Response (200):** [`GetCancelReasonDtoResponseListServiceResponse`](#schema-getcancelreasondtoresponselistserviceresponse)

---

#### `GET /api/Masters/claim/decision`
**คำอธิบาย:** API สำหรับ Get ข้อมูล Decision (ผลการพิจารณา)  
**operationId:** `GetDecision`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `decisionId` | query | integer (int32) | No |  |

**Response (200):** [`GetDecisionDtoResponseListServiceResponse`](#schema-getdecisiondtoresponselistserviceresponse)

---

#### `GET /api/Masters/claim/decision/reason`
**คำอธิบาย:** API สำหรับ Get ข้อมูล DecisionReason (สาเหตุจากผลการพิจารณา)  
**operationId:** `GetDecisionReason`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `decisionReasonId` | query | integer (int32) | No |  |
| `decisionId` | query | integer (int32) | No |  |

**Response (200):** [`GetDecisionReasonDtoResponseListServiceResponse`](#schema-getdecisionreasondtoresponselistserviceresponse)

---

#### `GET /api/Masters/claim/reject/reason`
**คำอธิบาย:** API สำหรับ Get ข้อมูล RejectReason (เหตุผลการปฏิเสธ)  
**operationId:** `GetRejectReason`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `rejectReasonId` | query | integer (int32) | No |  |

**Response (200):** [`GetRejectReasonDtoResponseListServiceResponse`](#schema-getrejectreasondtoresponselistserviceresponse)

---

#### `GET /api/Masters/claim/transaction-type`
**คำอธิบาย:** API สำหรับ Get ข้อมูล ClaimTransactionType (ประเภทธุรกรรมเคลม)  
**operationId:** `GetClaimTransactionType`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `claimTransactionTypeId` | query | integer (int32) | No |  |

**Response (200):** [`GetClaimTransactionTypeDtoResponseListServiceResponse`](#schema-getclaimtransactiontypedtoresponselistserviceresponse)

---

### document

#### `GET /api/Masters/document/recipient-type`
**คำอธิบาย:** API สำหรับ Get ข้อมูล DocumentRecipientType (ประเภทผู้รับเอกสาร)  
**operationId:** `GetDocumentRecipientType`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `documentRecipientTypeId` | query | integer (int32) | No |  |

**Response (200):** [`GetDocumentRecipientTypeDtoResponseListServiceResponse`](#schema-getdocumentrecipienttypedtoresponselistserviceresponse)

---

#### `GET /api/Masters/document/review/status`
**คำอธิบาย:** API สำหรับ Get ข้อมูล DocumentReviewStatus (สถานะการตรวจเอกสาร)  
**operationId:** `GetDocumentReviewStatus`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `documentReviewStatusId` | query | integer (int32) | No |  |

**Response (200):** [`GetDocumentReviewStatusDtoResponseListServiceResponse`](#schema-getdocumentreviewstatusdtoresponselistserviceresponse)

---

### bankaccount

#### `GET /api/Masters/bankaccount/relation/type`
**คำอธิบาย:** API สำหรับ Get ข้อมูล BankAccountRelationType (ประเภทความสัมพันธ์ของบัญชีธนาคาร) BankAccountRelationGroupId : 1 = PH , PA , ClaimMisc : 2 = Motor  
**operationId:** `GetBankAccountRelationType`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `bankAccountRelationTypeId` | query | integer (int32) | No |  |
| `bankAccountRelationGroupId` | query | integer (int32) | No |  |
| `productTypeId` | query | integer (int32) | No |  |

**Response (200):** [`GetBankAccountRelationTypeDtoResponseListServiceResponse`](#schema-getbankaccountrelationtypedtoresponselistserviceresponse)

---

### contactperson

#### `GET /api/Masters/contactperson/type`
**คำอธิบาย:** API สำหรับ Get ข้อมูล ContactPersonType (ประเภทผู้ติดต่อ) ContactPersonGroupId : 1 = PH , DeathClaim : 2 = PA, 3 = Motor  
**operationId:** `GetContactPersonType`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `contactPersonTypeId` | query | integer (int32) | No |  |
| `contactPersonGroupId` | query | integer (int32) | No |  |
| `productTypeId` | query | integer (int32) | No |  |

**Response (200):** [`GetContactPersonTypeDtoResponseListServiceResponse`](#schema-getcontactpersontypedtoresponselistserviceresponse)

---

### noncoveredreason

#### `GET /api/Masters/noncoveredreason`
**คำอธิบาย:** API สำหรับ Get ข้อมูล NonCoveredReason (สาเหตุที่ไม่คุ้มครอง)  
**operationId:** `GetNonCoveredReason`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `nonCoveredReasonId` | query | integer (int32) | No |  |
| `coverageTypeId` | query | integer (int32) | No |  |

**Response (200):** [`GetNonCoveredReasonDtoResponseListServiceResponse`](#schema-getnoncoveredreasondtoresponselistserviceresponse)

---

### incidenttype

#### `GET /api/Masters/incidenttype`
**คำอธิบาย:** API สำหรับ Get ข้อมูล IncidentType (เหตุของการเคลม)  
**operationId:** `GetIncidentType`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `incidentTypeId` | query | integer (int32) | No |  |

**Response (200):** [`IncidentTypeDtoResponseListServiceResponse`](#schema-incidenttypedtoresponselistserviceresponse)

---

#### `GET /api/Masters/incidenttype/mapping`
**คำอธิบาย:** API สำหรับ Get ข้อมูล CoverageType, MedicalType, CauseOfIncident By IncidentTypeId  
**operationId:** `GetIncidentTypeMapping`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `incidentTypeId` | query | integer (int32) | No |  |
| `claimSourceId` | query | integer (int32) | No |  |
| `productTypeId` | query | integer (int32) | No |  |
| `productCategoryCode` | query | string | No |  |
| `coverageTypeId` | query | integer (int32) | No |  |
| `medicalTypeId` | query | integer (int32) | No |  |
| `causeOfIncidentId` | query | integer (int32) | No |  |
| `isClaimContinue` | query | boolean | No |  |
| `initialClaimId` | query | string (uuid) | No |  |

**Response (200):** [`GetIncidentTypeMappingDtoResponseListServiceResponse`](#schema-getincidenttypemappingdtoresponselistserviceresponse)

---

### formatType

#### `GET /api/Masters/formatType`
**คำอธิบาย:** API สำหรับ Get ข้อมูล FormatType (ประการจ่าย)  
**operationId:** `GetFormatType`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `formatTypeId` | query | integer (int32) | No |  |

**Response (200):** [`FormatTypeDtoResponseListServiceResponse`](#schema-formattypedtoresponselistserviceresponse)

---

### chiefcomplaint

#### `GET /api/Masters/chiefcomplaint`
**คำอธิบาย:** API สำหรับ Get ข้อมูล ChiefComplaint (อาการสำคัญที่ผู้เคลมแจ้ง)  
**operationId:** `GetChiefComplaint`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `chiefComplaintId` | query | integer (int32) | No |  |

**Response (200):** [`GetChiefComplaintDtoResponseListServiceResponse`](#schema-getchiefcomplaintdtoresponselistserviceresponse)

---

### icd10

#### `GET /api/Masters/icd10`
**คำอธิบาย:** API สำหรับ Get ข้อมูล ICD10 (รหัสวินิจฉัยโรค)  
**operationId:** `GetICD10`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `ICD10Id` | query | integer (int32) | No |  |
| `ICD10Code` | query | string | No |  |
| `isTPA` | query | boolean | No |  |

**Response (200):** [`GetICD10DtoResponseListServiceResponse`](#schema-geticd10dtoresponselistserviceresponse)

---

## กลุ่ม HospitalBilling

### billing

#### `GET /api/billing/hospital/filter`
**คำอธิบาย:** ค้นหารายการวางบิลโรงพยาบาลและคืน dashboard counts ตาม BillingDetail.  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `StatusId` | query | integer (int32) | No |  |
| `SearchBy` | query | string | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |

**Response (200):** [`BillingListDtoServiceResponse`](#schema-billinglistdtoserviceresponse)

**Other status codes:** 400, 401, 403, 500

---

#### `GET /api/billing/hospital/{billingDetailId}`
**คำอธิบาย:** อ่านรายละเอียดรอบวางบิลสำหรับตรวจสอบหรือดูผลที่บันทึกไว้.  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `billingDetailId` | path | string (uuid) | Yes |  |

**Response (200):** [`BillingDetailDtoServiceResponse`](#schema-billingdetaildtoserviceresponse)

**Other status codes:** 401, 403, 404, 500

---

#### `GET /api/billing/hospital/{billingDetailId}/history`
**คำอธิบาย:** อ่านประวัติรอบวางบิลและ immutable review revisions ของ Case.  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `billingDetailId` | path | string (uuid) | Yes |  |

**Response (200):** [`BillingHistoryDtoServiceResponse`](#schema-billinghistorydtoserviceresponse)

**Other status codes:** 401, 403, 404, 500

---

#### `POST /api/billing/hospital/{billingDetailId}/submit`
**คำอธิบาย:** ยืนยันผลตรวจสอบและบันทึก immutable review revision พร้อม return request แบบ atomic.  
**Security:** OAuth2  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `billingDetailId` | path | string (uuid) | Yes |  |

**Request Body:** [`SubmitHospitalBillingDto`](#schema-submithospitalbillingdto)

**Response (200):** [`BillingSubmitResultDtoServiceResponse`](#schema-billingsubmitresultdtoserviceresponse)

**Other status codes:** 400, 401, 403, 404, 409, 500

---

## กลุ่ม IClaim

### check-eligible

#### `GET /api/IClaim/check-eligible`
**คำอธิบาย:** API สำหรับ Get ข้อมูล เช็คสิทธิ์ความคุ้มครอง  
**operationId:** `CheckEligible`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `searchIndex` | query | integer (int32) | No |  |
| `isSeachDetail` | query | boolean | No |  |
| `incidentDate` | query | string (date-time) | No |  |
| `schoolId` | query | integer (int32) | No |  |
| `provinceId` | query | integer (int32) | No |  |
| `incidentTypeId` | query | integer (int32) | No |  |
| `searchDetail` | query | string | No |  |
| `orderingField` | query | string | No |  |
| `ascendingOrder` | query | boolean | No |  |
| `Page` | query | integer (int32) | No |  |
| `recordsPerPage` | query | integer (int32) | No |  |
| `IClaimToken` | header | string | No |  |

**Response (200):** [`GetCustomerSearchDtoResponseListServiceResponse`](#schema-getcustomersearchdtoresponselistserviceresponse)

---

### customer

#### `POST /api/IClaim/customer/checkeligible`
**คำอธิบาย:** API สำหรับรวมข้อมูล Customer และ Benefit ตามประเภทการรักษา OPD หรือ IPD  
**operationId:** `CheckEligibleCustomer`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `IClaimToken` | header | string | No |  |

**Request Body:** [`CustomerCheckEligibleDtoRequest`](#schema-customercheckeligibledtorequest)

**Response (200):** [`CustomerCheckEligibleDtoResponseListServiceResponse`](#schema-customercheckeligibledtoresponselistserviceresponse)

---

### claim

#### `POST /api/IClaim/claim/history/check`
**คำอธิบาย:** API สำหรับตรวจสอบประวัติ Claim ของผู้เอาประกัน  
**operationId:** `CheckClaimHistory`  

**Parameters:**

| Name | In | Type | Required | Description |
|---|---|---|---|---|
| `IClaimToken` | header | string | No |  |

**Request Body:** [`CheckClaimHistoryDtoRequest`](#schema-checkclaimhistorydtorequest)

**Response (200):** [`CheckClaimHistoryDtoResponseListServiceResponse`](#schema-checkclaimhistorydtoresponselistserviceresponse)

---

## ภาคผนวก: Schemas ทั้งหมด

รวม 309 schemas ทั้งหมดจากไฟล์ swagger.json เรียงตามตัวอักษร

### accountDetail {#schema-accountdetail}
| Field | Type | Required | Description |
|---|---|---|---|
| `contactPerson` | string (nullable) | No | ประเภทผู้ติดต่อ |
| `accountNo` | string (nullable) | No |  |
| `accountName` | string (nullable) | No |  |
| `bankId` | integer (int32) (nullable) | No |  |
| `bankName` | string (nullable) | No |  |
| `createdAccoutDetailDate` | string (date-time) (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `phoneNumber` | string (nullable) | No |  |

### AdditionalTransferAccountDetailsResponseDto {#schema-additionaltransferaccountdetailsresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `toAccountName` | string (nullable) | No |  |
| `toAccountNo` | string (nullable) | No |  |
| `toBank` | string (nullable) | No |  |
| `toBankId` | integer (int32) (nullable) | No |  |
| `phoneNo` | string (nullable) | No |  |
| `claimantAccountRelationship` | string (nullable) | No |  |
| `casePayableId` | string (uuid) | No |  |

### AdditionalTransferAccountDetailsResponseDtoListServiceResponse {#schema-additionaltransferaccountdetailsresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`AdditionalTransferAccountDetailsResponseDto`](#schema-additionaltransferaccountdetailsresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### AdditionalTransferDetailsResponseDto {#schema-additionaltransferdetailsresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) | No |  |
| `caseId` | string (uuid) | No |  |
| `claimNo` | string (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `createdByUserName` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No | จำนวนเงินที่โอนแล้ว |
| `countItem` | integer (int32) | No | จำนวนรายการ |
| `additionalTransferLimit` | number (double) | No | จำนวนเงินโอนได้สูงสุด |
| `caseDetails` | array of [`CaseDetailDto`](#schema-casedetaildto) | No |  |
| `account` | [`accountDetail`](#schema-accountdetail) | No |  |

### AdditionalTransferDetailsResponseDtoServiceResponse {#schema-additionaltransferdetailsresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`AdditionalTransferDetailsResponseDto`](#schema-additionaltransferdetailsresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### AdditionalTransferMonitorRequestDto {#schema-additionaltransfermonitorrequestdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `branceId` | integer (int32) (nullable) | No |  |
| `paymentStatusId` | integer (int32) (nullable) | No |  |

### AdditionalTransferTransactionResponseDto {#schema-additionaltransfertransactionresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `remark` | string (nullable) | No |  |
| `transactionDate` | string (date-time) (nullable) | No |  |
| `claimTransactionTypeName` | string (nullable) | No |  |
| `createdByFullName` | string (nullable) | No |  |
| `amountTotal` | number (double) (nullable) | No |  |

### AdditionalTransferTransactionResponseDtoListServiceResponse {#schema-additionaltransfertransactionresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`AdditionalTransferTransactionResponseDto`](#schema-additionaltransfertransactionresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### AdjustmentReasonResponseDto {#schema-adjustmentreasonresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | integer (int32) | No |  |
| `name` | string (nullable) | No |  |

### AdjustmentReasonResponseDtoListServiceResponse {#schema-adjustmentreasonresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`AdjustmentReasonResponseDto`](#schema-adjustmentreasonresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### AdjustmentReasonResponseDtoServiceResponse {#schema-adjustmentreasonresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`AdjustmentReasonResponseDto`](#schema-adjustmentreasonresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### AllUserDtoResponse {#schema-alluserdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `userId` | integer (int32) (nullable) | No |  |
| `employeeId` | integer (int32) | No |  |
| `personId` | integer (int32) (nullable) | No |  |
| `employeeCode` | string (nullable) | No |  |
| `titleId` | integer (int32) (nullable) | No |  |
| `titleCode` | string (nullable) | No |  |
| `titleName` | string (nullable) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `personName` | string (nullable) | No |  |
| `teamId` | integer (int32) (nullable) | No |  |
| `teamCode` | string (nullable) | No |  |
| `teamName` | string (nullable) | No |  |
| `branchId` | integer (int32) (nullable) | No |  |
| `branchName` | string (nullable) | No |  |
| `employeeStatusId` | integer (int32) (nullable) | No |  |
| `employeeStatus` | string (nullable) | No |  |
| `positionId` | integer (int32) (nullable) | No |  |
| `departmentId` | integer (int32) (nullable) | No |  |
| `phoneNo` | string (nullable) | No |  |
| `displayName` | string (nullable) | No |  |

### AllUserDtoResponseListServiceResponse {#schema-alluserdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`AllUserDtoResponse`](#schema-alluserdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### ApproveCasePayableRequest {#schema-approvecasepayablerequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `payableAmount` | number (double) | Yes |  |
| `toBankId` | integer (int32) (nullable) | No |  |
| `toBankName` | string (nullable) | No |  |
| `toBankAccountNo` | string (nullable) | No |  |

### ApproveClaimDecisionDtoRequest {#schema-approveclaimdecisiondtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimDecision` | [`UpsertClaimDecisionDtoRequest`](#schema-upsertclaimdecisiondtorequest) | Yes |  |
| `calculateCaseCode` | string (uuid) | No |  |
| `isCombinedWithMedicalAll` | boolean | Yes |  |
| `casePayable` | [`ApproveCasePayableRequest`](#schema-approvecasepayablerequest) | Yes |  |
| `jsonDetail` | string | Yes |  |

### BankAccountRelationTypeResponseDto {#schema-bankaccountrelationtyperesponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | integer (int32) | No |  |
| `name` | string (nullable) | No |  |

### BankAccountRelationTypeResponseDtoListServiceResponse {#schema-bankaccountrelationtyperesponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`BankAccountRelationTypeResponseDto`](#schema-bankaccountrelationtyperesponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### BankAccountRelationTypeResponseDtoServiceResponse {#schema-bankaccountrelationtyperesponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`BankAccountRelationTypeResponseDto`](#schema-bankaccountrelationtyperesponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### BankInquiryDetailResponseDto {#schema-bankinquirydetailresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `transRefNo` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `payerBankName` | string (nullable) | No |  |
| `statusBank` | string (nullable) | No |  |
| `descriptionTH` | string (nullable) | No |  |
| `status` | string (nullable) | No |  |

### BankInquiryDetailResponseDtoServiceResponse {#schema-bankinquirydetailresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`BankInquiryDetailResponseDto`](#schema-bankinquirydetailresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### BeneficiarySaveClaimEditDraftRequest {#schema-beneficiarysaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `beneficiaryId` | string (uuid) | No |  |
| `policyBeneficiaryId` | integer (int32) | No |  |
| `titleId` | string (nullable) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `idCard` | string (nullable) | No |  |
| `phoneNo` | string (nullable) | No |  |
| `relationId` | integer (int32) (nullable) | No |  |
| `bankAccountRelationTypeId` | integer (int32) (nullable) | No |  |
| `bankId` | integer (int32) | No |  |
| `bankAccountNo` | string (nullable) | No |  |
| `bankAccountName` | string (nullable) | No |  |
| `payoutAmount` | number (double) | No |  |

### BeneficiaryV2Request {#schema-beneficiaryv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `policyBeneficiaryId` | integer (int32) | No |  |
| `titleId` | string (nullable) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `idCard` | string (nullable) | No |  |
| `phoneNo` | string (nullable) | No |  |
| `relationId` | integer (int32) (nullable) | No |  |
| `bankAccountRelationTypeId` | integer (int32) (nullable) | No |  |
| `bankId` | integer (int32) | No |  |
| `bankAccountNo` | string (nullable) | No |  |
| `bankAccountName` | string (nullable) | No |  |
| `payoutAmount` | number (double) | No |  |
| `payables` | array of [`CasePayableV2Request`](#schema-casepayablev2request) | No |  |

### BillingClaimDto {#schema-billingclaimdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `incidentTypeId` | integer (int32) (nullable) | No |  |
| `incidentDate` | string (date-time) (nullable) | No |  |
| `incidentTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `symptomOnsetDate` | string (date-time) (nullable) | No |  |
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `occurrenceDate` | string (date-time) (nullable) | No |  |
| `occurrenceTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `admissionDate` | string (date-time) (nullable) | No |  |
| `admissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `dischargeDate` | string (date-time) (nullable) | No |  |
| `dischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `hn` | string (nullable) | No |  |
| `vn` | string (nullable) | No |  |
| `an` | string (nullable) | No |  |
| `chiefComplaintId` | integer (int32) (nullable) | No |  |
| `chiefComplaint` | string (nullable) | No |  |
| `diagnosis1Id` | integer (int32) (nullable) | No |  |
| `diagnosis2Id` | integer (int32) (nullable) | No |  |
| `diagnosis3Id` | integer (int32) (nullable) | No |  |
| `note` | string (nullable) | No |  |

### BillingDetailDto {#schema-billingdetaildto}
| Field | Type | Required | Description |
|---|---|---|---|
| `billingRequestItemId` | string (uuid) (nullable) | No |  |
| `billingDetailId` | string (uuid) | No |  |
| `billingHeaderId` | string (uuid) | No |  |
| `caseId` | string (uuid) | No |  |
| `claimId` | string (uuid) | No |  |
| `billingRequestId` | string (nullable) | No |  |
| `billingRequestCode` | string (nullable) | No |  |
| `claimCode` | string (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `hospitalName` | string (nullable) | No |  |
| `provinceName` | string (nullable) | No |  |
| `submittedDate` | string (date-time) | No |  |
| `statusId` | integer (int32) | No |  |
| `version` | integer (int32) | No |  |
| `caseVersion` | integer (int32) | No | คงไว้เพื่อ backward compatibility; Submit ไม่ใช้ตรวจ concurrency ของ Case. |
| `claimVersion` | integer (int32) | No | คงไว้เพื่อ backward compatibility; Submit ไม่ใช้ตรวจ concurrency ของ Claim. |
| `rowVersion` | string (byte) (nullable) | No |  |
| `caseRowVersion` | string (byte) (nullable) | No | คงไว้เพื่อ backward compatibility; Submit ไม่ใช้ตรวจ concurrency ของ Case. |
| `claimRowVersion` | string (byte) (nullable) | No | คงไว้เพื่อ backward compatibility; Submit ไม่ใช้ตรวจ concurrency ของ Claim. |
| `reviewRemark` | string (nullable) | No |  |
| `insured` | [`BillingInsuredDto`](#schema-billinginsureddto) | No |  |
| `data` | [`BillingReviewDataDto`](#schema-billingreviewdatadto) | No |  |
| `totals` | [`BillingTotalsDto`](#schema-billingtotalsdto) | No |  |
| `requiredDocumentSubTypeIds` | array of integer (int32) | No |  |

### BillingDetailDtoServiceResponse {#schema-billingdetaildtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`BillingDetailDto`](#schema-billingdetaildto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### BillingDocumentDto {#schema-billingdocumentdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDocumentId` | string (uuid) | No |  |
| `documentId` | string (uuid) (nullable) | No |  |
| `documentSubTypeId` | integer (int32) (nullable) | No |  |
| `documentName` | string (nullable) | No |  |
| `fileCount` | integer (int32) (nullable) | No |  |
| `reviewStatusId` | integer (int32) (nullable) | No |  |
| `note` | string (nullable) | No |  |

### BillingExpenseDto {#schema-billingexpensedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseItemId` | string (uuid) (nullable) | No |  |
| `standardMedicalExpenseId` | integer (int32) | No |  |
| `itemName` | string (nullable) | No |  |
| `claimAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `nonCoveredReasonId` | integer (int32) (nullable) | No |  |
| `note` | string (nullable) | No |  |

### BillingHistoryDto {#schema-billinghistorydto}
| Field | Type | Required | Description |
|---|---|---|---|
| `rounds` | array of [`BillingRoundDto`](#schema-billingrounddto) | No |  |
| `revisions` | array of [`BillingRevisionDto`](#schema-billingrevisiondto) | No |  |

### BillingHistoryDtoServiceResponse {#schema-billinghistorydtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`BillingHistoryDto`](#schema-billinghistorydto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### BillingInsuredDto {#schema-billinginsureddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `name` | string (nullable) | No |  |
| `policyCode` | string (nullable) | No |  |
| `studentCard` | string (nullable) | No |  |
| `plan` | string (nullable) | No |  |
| `coverageStart` | string (date-time) (nullable) | No |  |
| `coverageEnd` | string (date-time) (nullable) | No |  |

### BillingListDto {#schema-billinglistdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `items` | array of [`BillingListItemDto`](#schema-billinglistitemdto) | No |  |
| `counts` | object (nullable) | No |  |

### BillingListDtoServiceResponse {#schema-billinglistdtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`BillingListDto`](#schema-billinglistdto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### BillingListItemDto {#schema-billinglistitemdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `billingDetailId` | string (uuid) | No |  |
| `billingHeaderId` | string (uuid) | No |  |
| `caseId` | string (uuid) | No |  |
| `billingRequestId` | string (nullable) | No |  |
| `claimCode` | string (nullable) | No |  |
| `insuredName` | string (nullable) | No |  |
| `hospitalName` | string (nullable) | No |  |
| `submittedDate` | string (date-time) | No |  |
| `treatmentDate` | string (date-time) (nullable) | No |  |
| `statusId` | integer (int32) | No |  |
| `amount` | number (double) | No |  |
| `canReview` | boolean | No |  |

### BillingMedicalDto {#schema-billingmedicaldto}
| Field | Type | Required | Description |
|---|---|---|---|
| `underlyingDiseaseDetail` | string (nullable) | No |  |
| `illnessDetail` | string (nullable) | No |  |
| `investigationResults` | string (nullable) | No |  |
| `isProcedurePerformed` | boolean (nullable) | No |  |
| `medicalLicenseNo` | string (nullable) | No |  |
| `physicianName` | string (nullable) | No |  |

### BillingReviewDataDto {#schema-billingreviewdatadto}
| Field | Type | Required | Description |
|---|---|---|---|
| `claim` | [`BillingClaimDto`](#schema-billingclaimdto) | Yes |  |
| `medical` | [`BillingMedicalDto`](#schema-billingmedicaldto) | Yes |  |
| `expenses` | array of [`BillingExpenseDto`](#schema-billingexpensedto) | Yes |  |
| `documents` | array of [`BillingDocumentDto`](#schema-billingdocumentdto) | Yes |  |

### BillingRevisionDto {#schema-billingrevisiondto}
| Field | Type | Required | Description |
|---|---|---|---|
| `revisionId` | string (uuid) | No |  |
| `version` | integer (int32) | No |  |
| `previousStatusId` | integer (int32) | No |  |
| `statusId` | integer (int32) | No |  |
| `reviewedDate` | string (date-time) | No |  |
| `reviewedByUserId` | integer (int32) | No |  |
| `returnStatus` | string (nullable) | No |  |
| `snapshot` | [`BillingDetailDto`](#schema-billingdetaildto) | No |  |

### BillingRoundDto {#schema-billingrounddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `billingDetailId` | string (uuid) | No |  |
| `billingRequestId` | string (nullable) | No |  |
| `statusId` | integer (int32) | No |  |
| `submittedDate` | string (date-time) | No |  |

### BillingSubmitResultDto {#schema-billingsubmitresultdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `billingDetailId` | string (uuid) | No |  |
| `revisionId` | string (uuid) | No |  |
| `statusId` | integer (int32) | No |  |
| `version` | integer (int32) | No |  |
| `netBillableAmount` | number (double) | No |  |
| `returnRequestId` | string (uuid) (nullable) | No |  |
| `returnStatus` | string (nullable) | No |  |

### BillingSubmitResultDtoServiceResponse {#schema-billingsubmitresultdtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`BillingSubmitResultDto`](#schema-billingsubmitresultdto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### BillingTotalsDto {#schema-billingtotalsdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `totalClaimedAmount` | number (double) | No |  |
| `totalDiscountAmount` | number (double) | No |  |
| `totalNonCoveredAmount` | number (double) | No |  |
| `insuranceDiscountAmount` | number (double) (nullable) | No |  |
| `customerDiscountAmount` | number (double) (nullable) | No |  |
| `netBillableAmount` | number (double) | No |  |

### CalculateCaseClaim {#schema-calculatecaseclaim}
| Field | Type | Required | Description |
|---|---|---|---|
| `productId` | integer (int32) (nullable) | No |  |
| `coverageTypeId` | integer (int32) | No |  |
| `medicalTypeId` | integer (int32) | No |  |
| `incidentTypeId` | integer (int32) | No |  |
| `occurrenceDate` | string (date-time) | No |  |
| `ipdCount` | integer (int32) | No |  |
| `icuCount` | integer (int32) | No |  |
| `continueClaimNo` | string (nullable) | No |  |
| `expenseList` | array of [`CalculateCaseExpense`](#schema-calculatecaseexpense) | No |  |
| `disabilityList` | array of [`CalculateCaseDisability`](#schema-calculatecasedisability) | No |  |

### CalculateCaseClaimDtoRequest {#schema-calculatecaseclaimdtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseAdjudicationId` | string (uuid) (nullable) | No |  |
| `isSimulateCase` | boolean | No |  |
| `isCheckIncludeCompensate` | boolean | No |  |
| `isCheckIncludeCompensateAll` | boolean | No |  |
| `jsonDetail` | [`CalculateCaseClaim`](#schema-calculatecaseclaim) | No |  |

### CalculateCaseClaimDtoResponse {#schema-calculatecaseclaimdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseAdjudicationId` | string (uuid) (nullable) | No |  |
| `calculateCaseCode` | string (uuid) (nullable) | No |  |
| `medicalExpense` | array of [`MedicalExpenseList`](#schema-medicalexpenselist) | No |  |
| `compensateExpense` | array of [`CompensateExpenseList`](#schema-compensateexpenselist) | No |  |
| `disabilityExpense` | array of [`DisabilityExpenseList`](#schema-disabilityexpenselist) | No |  |
| `compensateNet` | number (double) | No |  |
| `compensateInclude` | number (double) | No |  |
| `compensateRemain` | number (double) | No |  |
| `medicalNet` | number (double) | No |  |
| `medicalCoverPay` | number (double) | No |  |
| `medicalPay` | number (double) | No |  |
| `medicalCompensateInclude` | number (double) | No |  |
| `medicalUnpay` | number (double) | No |  |

### CalculateCaseClaimDtoResponseServiceResponse {#schema-calculatecaseclaimdtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`CalculateCaseClaimDtoResponse`](#schema-calculatecaseclaimdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### CalculateCaseDisability {#schema-calculatecasedisability}
| Field | Type | Required | Description |
|---|---|---|---|
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `bodyPartId` | integer (int32) | No |  |
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `nonCoverAmount` | number (double) | No |  |
| `disabilityPercent` | number (double) | No |  |
| `reasonId` | integer (int32) (nullable) | No |  |
| `remark` | string (nullable) | No |  |

### CalculateCaseDisabilityDtoResponse {#schema-calculatecasedisabilitydtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `policyCode` | string (nullable) | No |  |
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `bodyPartId` | integer (int32) (nullable) | No |  |
| `bodyPartName` | string (nullable) | No |  |
| `benefitPercent` | number (double) (nullable) | No |  |
| `benefitId` | integer (int32) (nullable) | No |  |
| `benefitName` | string (nullable) | No |  |
| `benefitUnitName` | string (nullable) | No |  |
| `pricePerUnit` | number (double) (nullable) | No |  |
| `maxPrice` | number (double) (nullable) | No |  |
| `sumUsedAmount` | number (double) (nullable) | No |  |

### CalculateCaseDisabilityDtoResponseServiceResponse {#schema-calculatecasedisabilitydtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`CalculateCaseDisabilityDtoResponse`](#schema-calculatecasedisabilitydtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### CalculateCaseExpense {#schema-calculatecaseexpense}
| Field | Type | Required | Description |
|---|---|---|---|
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `description` | string (nullable) | No |  |
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `nonCoverAmount` | number (double) | No |  |
| `reasonId` | integer (int32) (nullable) | No |  |
| `remark` | string (nullable) | No |  |

### CaseAdjudicationSaveClaimEditDraftRequest {#schema-caseadjudicationsaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `decisionId` | integer (int32) | No |  |
| `decisionDate` | string (date-time) | No |  |
| `approvedAdmissionDate` | string (date-time) (nullable) | No |  |
| `approvedAdmissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `approvedDischargeDate` | string (date-time) (nullable) | No |  |
| `approvedDischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `coveredAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `compensateAmount` | number (double) | No |  |
| `approvedMedicalAmount` | number (double) | No |  |
| `approvedCompensateAmount` | number (double) | No |  |
| `patientPayAmount` | number (double) | No |  |
| `isExgratia` | boolean | No |  |
| `exgratiaAmount` | number (double) | No |  |
| `deductibleAmount` | number (double) | No |  |
| `coPayAmount` | number (double) | No |  |
| `coInsuranceAmount` | number (double) | No |  |
| `rejectReasonId` | integer (int32) (nullable) | No |  |
| `rejectDate` | string (date-time) (nullable) | No |  |
| `isLatest` | boolean | No |  |
| `approvedIPDDayCount` | integer (int32) | No |  |
| `approvedICUDayCount` | integer (int32) | No |  |
| `decisionReasonId` | integer (int32) (nullable) | No |  |
| `decisionRemark` | string (nullable) | No |  |
| `caseItemAdjudications` | array of [`CaseItemAdjudicationSaveClaimEditDraftRequest`](#schema-caseitemadjudicationsaveclaimeditdraftrequest) | No |  |

### CaseAssessmentSaveClaimEditDraftRequest {#schema-caseassessmentsaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `isDocumentComplete` | boolean | No |  |
| `documentReceivedDate` | string (date-time) | No |  |
| `documentCompleteDate` | string (date-time) | No |  |
| `isFraudSuspect` | boolean | No |  |
| `documentReceivedByUserId` | integer (int32) (nullable) | No |  |
| `documentReceivedByUserCode` | string (nullable) | No |  |
| `documentReceivedByUserName` | string (nullable) | No |  |

### CaseAssessmentV2Request {#schema-caseassessmentv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `isDocumentComplete` | boolean | No |  |
| `documentReceivedDate` | string (date-time) (nullable) | No |  |
| `documentCompleteDate` | string (date-time) (nullable) | No |  |
| `isFraudSuspect` | boolean | No |  |
| `documentReceivedByUserId` | integer (int32) (nullable) | No |  |
| `documentReceivedByUserCode` | string (nullable) | No |  |
| `documentReceivedByUserName` | string (nullable) | No |  |

### CaseContactV2Request {#schema-casecontactv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `contactPersonTypeId` | integer (int32) (nullable) | No |  |
| `contactPersonName` | string (nullable) | No |  |
| `contactPhoneNo` | string (nullable) | No |  |

### CaseDeathSaveClaimEditDraftRequest {#schema-casedeathsaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDeathId` | string (uuid) | No |  |
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |
| `deathDate` | string (date-time) | No |  |
| `deathTime` | [`TimeSpan`](#schema-timespan) | No |  |

### CaseDeathV2Request {#schema-casedeathv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |
| `deathDate` | string (date-time) | No |  |
| `deathTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `placeOfDeathId` | integer (int32) | No |  |
| `placeOfDeathDetail` | string (nullable) | No |  |

### CaseDetailDto {#schema-casedetaildto}
| Field | Type | Required | Description |
|---|---|---|---|
| `customerName` | string (nullable) | No |  |
| `coverageTypeNameTH` | string (nullable) | No | ประเภทความคุ้มครอง |
| `caseNo` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No | จำนวนเงินที่โอนแล้ว |

### CaseDisabilitySaveClaimEditDraftRequest {#schema-casedisabilitysaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDisabilityId` | string (uuid) | No |  |
| `bodyPartId` | integer (int32) (nullable) | No |  |
| `disabilityTypeId` | integer (int32) (nullable) | No |  |
| `disabilityLevel` | integer (int32) (nullable) | No |  |
| `disabilityPercent` | integer (int32) (nullable) | No |  |

### CaseDisabilityV2Request {#schema-casedisabilityv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `bodyPartId` | integer (int32) (nullable) | No |  |
| `disabilityTypeId` | integer (int32) (nullable) | No |  |
| `disabilityLevel` | integer (int32) (nullable) | No |  |
| `disabilityPercent` | integer (int32) (nullable) | No |  |
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |

### CaseDocumentDetailSaveClaimEditDraftRequest {#schema-casedocumentdetailsaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDocumentDetailId` | string (uuid) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `fullName` | string (nullable) | No |  |
| `hospitalName` | string (nullable) | No |  |
| `receiptAdmissionDate` | string (date-time) (nullable) | No |  |
| `receiptNumber` | string (nullable) | No |  |
| `receiptAmount` | number (double) (nullable) | No |  |
| `ocrDocumentTypeId` | integer (int32) (nullable) | No |  |
| `ocrResult` | string (nullable) | No |  |

### CaseDocumentDetailV2Request {#schema-casedocumentdetailv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `fullName` | string (nullable) | No |  |
| `hospitalName` | string (nullable) | No |  |
| `receiptAdmissionDate` | string (date-time) (nullable) | No |  |
| `receiptNumber` | string (nullable) | No |  |
| `receiptAmount` | number (double) (nullable) | No |  |
| `ocrDocumentTypeId` | integer (int32) (nullable) | No |  |
| `ocrResult` | string (nullable) | No |  |

### CaseDocumentSaveClaimEditDraftRequest {#schema-casedocumentsaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDocumentId` | string (uuid) | No |  |
| `documentId` | string (uuid) | No |  |
| `documentNo` | string (nullable) | No |  |
| `documentSubTypeId` | integer (int32) (nullable) | No |  |
| `caseDocumentDetail` | array of [`CaseDocumentDetailSaveClaimEditDraftRequest`](#schema-casedocumentdetailsaveclaimeditdraftrequest) | No |  |

### CaseDocumentV2Request {#schema-casedocumentv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `documentId` | string (uuid) | No |  |
| `documentNo` | string (nullable) | No |  |
| `claimDocumentTypeId` | integer (int32) (nullable) | No |  |
| `documentSubTypeId` | integer (int32) | No |  |
| `details` | array of [`CaseDocumentDetailV2Request`](#schema-casedocumentdetailv2request) | No |  |

### CaseItemAdjudicationSaveClaimEditDraftRequest {#schema-caseitemadjudicationsaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseItemAdjusication` | string (uuid) (nullable) | No |  |
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `caseItemId` | string (uuid) (nullable) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `eligibleAmount` | number (double) | No |  |
| `approvedAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `excessAmount` | number (double) | No |  |

### CaseItemSaveClaimEditDraftRequest {#schema-caseitemsaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseItemId` | string (uuid) (nullable) | No |  |
| `inputToStandardMappingId` | integer (int32) | No |  |
| `standardMedicalExpenseId` | integer (int32) | No |  |
| `quantity` | integer (int32) | No |  |
| `perUnit` | integer (int32) | No |  |
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `nonCoveredReasonId` | integer (int32) (nullable) | No |  |

### CaseItemV2Request {#schema-caseitemv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `inputToStandardMappingId` | integer (int32) | No |  |
| `standardMedicalExpenseId` | integer (int32) | No |  |
| `quantity` | integer (int32) | No |  |
| `perUnit` | integer (int32) | No |  |
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `nonCoveredReasonId` | integer (int32) | No |  |

### CasePayableV2Request {#schema-casepayablev2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `payableCategoryId` | integer (int32) | No |  |

### CaseRefundApproveDetailResponseDto {#schema-caserefundapprovedetailresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimNo` | string (nullable) | No | เลข CL |
| `caseNo` | string (nullable) | No | เลขที่ case |
| `branchName` | string (nullable) | No | สาขา |
| `insuredName` | string (nullable) | No | ชื่อ - สกุล ผู้เอาประกัน |
| `createdBy` | string (nullable) | No | ผู้ทำรายการ |
| `refundCount` | integer (int32) | No | จำนวนเคสคืนเงิน |
| `transferAmount` | number (double) (nullable) | No | จำนวนเงินที่แจ้งโอน |
| `totalRefundAmount` | number (double) (nullable) | No | โอนคืนรวม |
| `remainingAmount` | number (double) (nullable) | No | คงเหลือ |
| `refundDate` | string (date-time) (nullable) | No | วันที่/เวลาที่โอนเงินคืน |
| `caseRefundId` | string (uuid) | No |  |

### CaseRefundApproveDetailResponseDtoServiceResponse {#schema-caserefundapprovedetailresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`CaseRefundApproveDetailResponseDto`](#schema-caserefundapprovedetailresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### CaseRefundApproveUpdateStatusRequestDto {#schema-caserefundapproveupdatestatusrequestdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseRefundId` | string (uuid) | No |  |
| `caseRefundStatusId` | integer (int32) | No |  |
| `caseRefundRejectReasonId` | integer (int32) (nullable) | No |  |
| `caseRefundRejectReasonRemark` | string (nullable) | No |  |

### CaseRefundApproveUpdateStatusResponseDto {#schema-caserefundapproveupdatestatusresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |

### CaseRefundApproveUpdateStatusResponseDtoServiceResponse {#schema-caserefundapproveupdatestatusresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`CaseRefundApproveUpdateStatusResponseDto`](#schema-caserefundapproveupdatestatusresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### CaseRefundRejectReasonResponseDto {#schema-caserefundrejectreasonresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | integer (int32) | No |  |
| `name` | string (nullable) | No |  |

### CaseRefundRejectReasonResponseDtoListServiceResponse {#schema-caserefundrejectreasonresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`CaseRefundRejectReasonResponseDto`](#schema-caserefundrejectreasonresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### CaseRegistrationV2Request {#schema-caseregistrationv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `notificationDate` | string (date-time) (nullable) | No |  |
| `notifyBy` | string (nullable) | No |  |
| `initialCoverageTypeId` | integer (int32) (nullable) | No |  |
| `initialCaseAmount` | number (double) | No |  |
| `initialCaseSourceId` | integer (int32) (nullable) | No |  |
| `preAuthId` | string (uuid) (nullable) | No |  |
| `initialMedicalTypeId` | integer (int32) (nullable) | No |  |

### CaseSaveClaimEditDraftRequest {#schema-casesaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `occurrenceDate` | string (date-time) (nullable) | No |  |
| `occurrenceTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `admissionDate` | string (date-time) (nullable) | No |  |
| `admissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `dischargeDate` | string (date-time) (nullable) | No |  |
| `dischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `caseAmount` | number (double) | No |  |
| `latestApprovedAmount` | number (double) | No |  |
| `latestNonCoveredAmount` | number (double) | No |  |
| `latestPatientPayAmount` | number (double) | No |  |
| `cancelReasonId` | integer (int32) (nullable) | No |  |
| `cancelDate` | string (date-time) (nullable) | No |  |
| `isCaseDisability` | boolean | No |  |
| `hospitalId` | integer (int32) (nullable) | No |  |
| `hn` | string (nullable) | No |  |
| `an` | string (nullable) | No |  |
| `vn` | string (nullable) | No |  |
| `chiefComplaintId` | integer (int32) (nullable) | No |  |
| `chiefComplaintCustom` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `icD10_1stId` | integer (int32) (nullable) | No |  |
| `icD10_2ndId` | integer (int32) (nullable) | No |  |
| `icD10_3rdId` | integer (int32) (nullable) | No |  |
| `icD10_4thId` | integer (int32) (nullable) | No |  |
| `icD10_5thId` | integer (int32) (nullable) | No |  |
| `icD10_6thId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nplAmount` | number (double) (nullable) | No |  |
| `insuranceDiscountAmount` | number (double) (nullable) | No |  |
| `customerDiscountAmount` | number (double) (nullable) | No |  |
| `caseItem` | array of [`CaseItemSaveClaimEditDraftRequest`](#schema-caseitemsaveclaimeditdraftrequest) | No |  |
| `caseAssessment` | [`CaseAssessmentSaveClaimEditDraftRequest`](#schema-caseassessmentsaveclaimeditdraftrequest) | No |  |
| `caseAdjudication` | [`CaseAdjudicationSaveClaimEditDraftRequest`](#schema-caseadjudicationsaveclaimeditdraftrequest) | No |  |
| `caseDeath` | array of [`CaseDeathSaveClaimEditDraftRequest`](#schema-casedeathsaveclaimeditdraftrequest) | No |  |
| `caseDisability` | array of [`CaseDisabilitySaveClaimEditDraftRequest`](#schema-casedisabilitysaveclaimeditdraftrequest) | No |  |
| `beneficiary` | array of [`BeneficiarySaveClaimEditDraftRequest`](#schema-beneficiarysaveclaimeditdraftrequest) | No |  |
| `caseDocument` | array of [`CaseDocumentSaveClaimEditDraftRequest`](#schema-casedocumentsaveclaimeditdraftrequest) | No |  |

### CaseServicePersonV2Request {#schema-caseservicepersonv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `servicePersonByUserId` | integer (int32) | No |  |
| `servicePersonByUserCode` | string (nullable) | No |  |
| `servicePersonByUserName` | string (nullable) | No |  |
| `zebraId` | integer (int32) | No |  |
| `zebraCode` | string (nullable) | No |  |
| `zebraNo` | string (nullable) | No |  |
| `employeeCode` | string (nullable) | No |  |
| `employeeName` | string (nullable) | No |  |

### CaseV2Request {#schema-casev2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `occurrenceDate` | string (date) (nullable) | No |  |
| `occurrenceTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `admissionDate` | string (date) (nullable) | No |  |
| `admissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `dischargeDate` | string (date) (nullable) | No |  |
| `dischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `caseAmount` | number (double) | No |  |
| `latestApprovedAmount` | number (double) | No |  |
| `latestNonCoveredAmount` | number (double) | No |  |
| `latestPatientPayAmount` | number (double) | No |  |
| `isCaseDisability` | boolean | No |  |
| `hospitalId` | integer (int32) (nullable) | No |  |
| `hn` | string (nullable) | No |  |
| `an` | string (nullable) | No |  |
| `vn` | string (nullable) | No |  |
| `chiefComplaintId` | integer (int32) (nullable) | No |  |
| `chiefComplaintCustom` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `icD10_1stId` | integer (int32) (nullable) | No |  |
| `icD10_2ndId` | integer (int32) (nullable) | No |  |
| `icD10_3rdId` | integer (int32) (nullable) | No |  |
| `icD10_4thId` | integer (int32) (nullable) | No |  |
| `icD10_5thId` | integer (int32) (nullable) | No |  |
| `icD10_6thId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nplAmount` | number (double) (nullable) | No |  |
| `insuranceDiscountAmount` | number (double) (nullable) | No |  |
| `customerDiscountAmount` | number (double) (nullable) | No |  |
| `items` | array of [`CaseItemV2Request`](#schema-caseitemv2request) | No |  |
| `registrations` | array of [`CaseRegistrationV2Request`](#schema-caseregistrationv2request) | No |  |
| `assessments` | array of [`CaseAssessmentV2Request`](#schema-caseassessmentv2request) | No |  |
| `deaths` | array of [`CaseDeathV2Request`](#schema-casedeathv2request) | No |  |
| `disabilities` | array of [`CaseDisabilityV2Request`](#schema-casedisabilityv2request) | No |  |
| `documents` | array of [`CaseDocumentV2Request`](#schema-casedocumentv2request) | No |  |
| `contacts` | array of [`CaseContactV2Request`](#schema-casecontactv2request) | No |  |
| `servicePersons` | array of [`CaseServicePersonV2Request`](#schema-caseservicepersonv2request) | No |  |
| `beneficiaries` | array of [`BeneficiaryV2Request`](#schema-beneficiaryv2request) | No |  |

### CheckClaimHistoryDtoRequest {#schema-checkclaimhistorydtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `searchOption` | string | Yes |  |
| `cidPassport` | string (nullable) | No |  |
| `id` | string (nullable) | No |  |
| `certNo` | string (nullable) | No |  |
| `policyNumber` | string (nullable) | No |  |
| `priviledgeCardNo` | string (nullable) | No |  |
| `hospitalCode` | string | Yes |  |
| `userName` | string | Yes |  |

### CheckClaimHistoryDtoResponse {#schema-checkclaimhistorydtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `customer` | [`CustomerCheckEligibleCustomerDto`](#schema-customercheckeligiblecustomerdto) | No |  |
| `history` | array of [`GetClaimHistoryDtoResponse`](#schema-getclaimhistorydtoresponse) | No |  |

### CheckClaimHistoryDtoResponseListServiceResponse {#schema-checkclaimhistorydtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`CheckClaimHistoryDtoResponse`](#schema-checkclaimhistorydtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### ClaimDecisionOverviewDtoResponse {#schema-claimdecisionoverviewdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseAdjudicationId` | string (uuid) | No |  |
| `decisionId` | integer (int32) (nullable) | No |  |
| `decisionName` | string (nullable) | No |  |
| `decisionReasonId` | integer (int32) (nullable) | No |  |
| `decisionReasonName` | string (nullable) | No |  |
| `rejectReasonId` | integer (int32) (nullable) | No |  |
| `rejectReasonName` | string (nullable) | No |  |
| `reason` | string (nullable) | No | ข้อความเหตุผลที่ใช้แสดงผล โดยเลือก RejectReason ก่อน DecisionReason เมื่อมีข้อมูล |
| `decisionRemark` | string (nullable) | No |  |
| `decisionDate` | string (date-time) (nullable) | No |  |
| `coveredAmount` | number (double) (nullable) | No |  |
| `nonCoveredAmount` | number (double) (nullable) | No |  |
| `approvedMedicalAmount` | number (double) (nullable) | No |  |
| `patientPayAmount` | number (double) (nullable) | No |  |

### ClaimEditDraftBeneficiaryPayloadDto {#schema-claimeditdraftbeneficiarypayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `beneficiaryId` | string (uuid) | No |  |
| `policyBeneficiaryId` | integer (int32) | No |  |
| `titleId` | string (nullable) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `idCard` | string (nullable) | No |  |
| `phoneNo` | string (nullable) | No |  |
| `relationId` | integer (int32) (nullable) | No |  |
| `bankAccountRelationTypeId` | integer (int32) (nullable) | No |  |
| `bankId` | integer (int32) | No |  |
| `bankAccountNo` | string (nullable) | No |  |
| `bankAccountName` | string (nullable) | No |  |
| `payoutAmount` | number (double) | No |  |

### ClaimEditDraftCaseAdjudicationPayloadDto {#schema-claimeditdraftcaseadjudicationpayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `decisionId` | integer (int32) | No |  |
| `decisionDate` | string (date-time) | No |  |
| `approvedAdmissionDate` | string (date-time) (nullable) | No |  |
| `approvedAdmissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `approvedDischargeDate` | string (date-time) (nullable) | No |  |
| `approvedDischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `coveredAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `compensateAmount` | number (double) | No |  |
| `approvedMedicalAmount` | number (double) | No |  |
| `approvedCompensateAmount` | number (double) | No |  |
| `patientPayAmount` | number (double) | No |  |
| `isExgratia` | boolean | No |  |
| `exgratiaAmount` | number (double) | No |  |
| `deductibleAmount` | number (double) | No |  |
| `coPayAmount` | number (double) | No |  |
| `coInsuranceAmount` | number (double) | No |  |
| `rejectReasonId` | integer (int32) (nullable) | No |  |
| `rejectDate` | string (date-time) (nullable) | No |  |
| `isLatest` | boolean | No |  |
| `approvedIPDDayCount` | integer (int32) | No |  |
| `approvedICUDayCount` | integer (int32) | No |  |
| `decisionReasonId` | integer (int32) (nullable) | No |  |
| `decisionRemark` | string (nullable) | No |  |
| `caseItemAdjudications` | array of [`ClaimEditDraftCaseItemAdjudicationPayloadDto`](#schema-claimeditdraftcaseitemadjudicationpayloaddto) | No |  |

### ClaimEditDraftCaseAssessmentPayloadDto {#schema-claimeditdraftcaseassessmentpayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `isDocumentComplete` | boolean | No |  |
| `documentReceivedDate` | string (date-time) | No |  |
| `documentCompleteDate` | string (date-time) | No |  |
| `isFraudSuspect` | boolean | No |  |
| `documentReceivedByUserId` | integer (int32) (nullable) | No |  |
| `documentReceivedByUserCode` | string (nullable) | No |  |
| `documentReceivedByUserName` | string (nullable) | No |  |

### ClaimEditDraftCaseDeathPayloadDto {#schema-claimeditdraftcasedeathpayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDeathId` | string (uuid) | No |  |
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |
| `deathDate` | string (date-time) | No |  |
| `deathTime` | [`TimeSpan`](#schema-timespan) | No |  |

### ClaimEditDraftCaseDisabilityPayloadDto {#schema-claimeditdraftcasedisabilitypayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDisabilityId` | string (uuid) | No |  |
| `bodyPartId` | integer (int32) (nullable) | No |  |
| `disabilityTypeId` | integer (int32) (nullable) | No |  |
| `disabilityLevel` | integer (int32) (nullable) | No |  |
| `disabilityPercent` | integer (int32) (nullable) | No |  |

### ClaimEditDraftCaseDocumentDetailPayloadDto {#schema-claimeditdraftcasedocumentdetailpayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDocumentDetailId` | string (uuid) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `fullName` | string (nullable) | No |  |
| `hospitalName` | string (nullable) | No |  |
| `receiptAdmissionDate` | string (date-time) (nullable) | No |  |
| `receiptNumber` | string (nullable) | No |  |
| `receiptAmount` | number (double) (nullable) | No |  |
| `ocrDocumentTypeId` | integer (int32) (nullable) | No |  |
| `ocrResult` | string (nullable) | No |  |

### ClaimEditDraftCaseDocumentPayloadDto {#schema-claimeditdraftcasedocumentpayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDocumentId` | string (uuid) | No |  |
| `documentId` | string (uuid) | No |  |
| `documentNo` | string (nullable) | No |  |
| `documentSubTypeId` | integer (int32) (nullable) | No |  |
| `caseDocumentDetail` | array of [`ClaimEditDraftCaseDocumentDetailPayloadDto`](#schema-claimeditdraftcasedocumentdetailpayloaddto) | No |  |

### ClaimEditDraftCaseItemAdjudicationPayloadDto {#schema-claimeditdraftcaseitemadjudicationpayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseItemAdjusication` | string (uuid) (nullable) | No |  |
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `caseItemId` | string (uuid) (nullable) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `eligibleAmount` | number (double) | No |  |
| `approvedAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `excessAmount` | number (double) | No |  |

### ClaimEditDraftCaseItemPayloadDto {#schema-claimeditdraftcaseitempayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseItemId` | string (uuid) (nullable) | No |  |
| `inputToStandardMappingId` | integer (int32) | No |  |
| `standardMedicalExpenseId` | integer (int32) | No |  |
| `quantity` | integer (int32) | No |  |
| `perUnit` | integer (int32) | No |  |
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `nonCoveredReasonId` | integer (int32) (nullable) | No |  |

### ClaimEditDraftCasePayloadDto {#schema-claimeditdraftcasepayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `occurrenceDate` | string (date-time) (nullable) | No |  |
| `occurrenceTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `admissionDate` | string (date-time) (nullable) | No |  |
| `admissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `dischargeDate` | string (date-time) (nullable) | No |  |
| `dischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `caseAmount` | number (double) | No |  |
| `latestApprovedAmount` | number (double) | No |  |
| `latestNonCoveredAmount` | number (double) | No |  |
| `latestPatientPayAmount` | number (double) | No |  |
| `cancelReasonId` | integer (int32) (nullable) | No |  |
| `cancelDate` | string (date-time) (nullable) | No |  |
| `isCaseDisability` | boolean | No |  |
| `hospitalId` | integer (int32) (nullable) | No |  |
| `hn` | string (nullable) | No |  |
| `an` | string (nullable) | No |  |
| `vn` | string (nullable) | No |  |
| `chiefComplaintId` | integer (int32) (nullable) | No |  |
| `chiefComplaintCustom` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `icD10_1stId` | integer (int32) (nullable) | No |  |
| `icD10_2ndId` | integer (int32) (nullable) | No |  |
| `icD10_3rdId` | integer (int32) (nullable) | No |  |
| `icD10_4thId` | integer (int32) (nullable) | No |  |
| `icD10_5thId` | integer (int32) (nullable) | No |  |
| `icD10_6thId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nplAmount` | number (double) (nullable) | No |  |
| `insuranceDiscountAmount` | number (double) (nullable) | No |  |
| `customerDiscountAmount` | number (double) (nullable) | No |  |
| `caseItem` | array of [`ClaimEditDraftCaseItemPayloadDto`](#schema-claimeditdraftcaseitempayloaddto) | No |  |
| `caseAssessment` | [`ClaimEditDraftCaseAssessmentPayloadDto`](#schema-claimeditdraftcaseassessmentpayloaddto) | No |  |
| `caseAdjudication` | [`ClaimEditDraftCaseAdjudicationPayloadDto`](#schema-claimeditdraftcaseadjudicationpayloaddto) | No |  |
| `caseDeath` | array of [`ClaimEditDraftCaseDeathPayloadDto`](#schema-claimeditdraftcasedeathpayloaddto) | No |  |
| `caseDisability` | array of [`ClaimEditDraftCaseDisabilityPayloadDto`](#schema-claimeditdraftcasedisabilitypayloaddto) | No |  |
| `beneficiary` | array of [`ClaimEditDraftBeneficiaryPayloadDto`](#schema-claimeditdraftbeneficiarypayloaddto) | No |  |
| `caseDocument` | array of [`ClaimEditDraftCaseDocumentPayloadDto`](#schema-claimeditdraftcasedocumentpayloaddto) | No |  |

### ClaimEditDraftPayloadDto {#schema-claimeditdraftpayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) | No |  |
| `caseId` | string (uuid) | No |  |
| `incidentTypeId` | integer (int32) | No |  |
| `incidentDate` | string (date-time) | No |  |
| `incidentTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `accidentPlace` | string (nullable) | No |  |
| `accidentDescription` | string (nullable) | No |  |
| `draftStep` | integer (int32) | No |  |
| `case` | [`ClaimEditDraftCasePayloadDto`](#schema-claimeditdraftcasepayloaddto) | No |  |
| `claimEditDraft` | [`ClaimEditDraftStatusPayloadDto`](#schema-claimeditdraftstatuspayloaddto) | No |  |

### ClaimEditDraftSaveClaimEditDraftRequest {#schema-claimeditdraftsaveclaimeditdraftrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `baseClaimVersion` | integer (int32) | Yes |  |
| `baseCaseVersion` | integer (int32) | Yes |  |
| `claimEditDraftStatusId` | integer (int32) | No |  |

### ClaimEditDraftStatusPayloadDto {#schema-claimeditdraftstatuspayloaddto}
| Field | Type | Required | Description |
|---|---|---|---|
| `baseClaimVersion` | integer (int32) | No |  |
| `baseCaseVersion` | integer (int32) | No |  |
| `claimEditDraftStatusId` | integer (int32) | No |  |

### ClaimV2Request {#schema-claimv2request}
| Field | Type | Required | Description |
|---|---|---|---|
| `policyCode` | string | Yes |  |
| `policyNo` | string (nullable) | No |  |
| `certificateNo` | string (nullable) | No |  |
| `customerDetailId` | string (uuid) | No |  |
| `customerName` | string | Yes |  |
| `incidentTypeId` | integer (int32) (nullable) | No |  |
| `incidentDate` | string (date) | No |  |
| `incidentTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `accidentPlace` | string (nullable) | No |  |
| `accidentDescription` | string (nullable) | No |  |
| `cases` | array of [`CaseV2Request`](#schema-casev2request) | Yes |  |

### CompensateExpenseList {#schema-compensateexpenselist}
| Field | Type | Required | Description |
|---|---|---|---|
| `benefitId` | integer (int32) (nullable) | No |  |
| `benefitName` | string (nullable) | No |  |
| `net` | number (double) | No |  |
| `cover` | number (double) | No |  |
| `unCover` | number (double) | No |  |
| `pay` | number (double) | No |  |
| `unPay` | number (double) | No |  |
| `countDay` | integer (int32) | No |  |
| `dayOfUnit` | number (double) | No |  |

### CreateCaseAdjudicationDtoRequest {#schema-createcaseadjudicationdtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `requestId` | string (uuid) | Yes |  |
| `caseId` | string (uuid) | Yes |  |

### CreateCaseAdjudicationDtoResponse {#schema-createcaseadjudicationdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `isResult` | boolean | No |  |
| `result` | string (nullable) | No |  |
| `msg` | string (nullable) | No |  |
| `caseId` | string (uuid) | No |  |
| `caseAdjudicationId` | string (uuid) | No |  |
| `versionNo` | integer (int32) (nullable) | No |  |
| `caseItemAdjudicationCount` | integer (int32) | No |  |
| `isExisting` | boolean | No |  |

### CreateCaseAdjudicationDtoResponseServiceResponse {#schema-createcaseadjudicationdtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`CreateCaseAdjudicationDtoResponse`](#schema-createcaseadjudicationdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### CreateContinuedClaimDtoRequest {#schema-createcontinuedclaimdtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `requestId` | string (uuid) | Yes |  |
| `claimId` | string (uuid) | Yes |  |
| `cases` | array of [`CaseV2Request`](#schema-casev2request) | Yes |  |
| `createdByBranchId` | integer (int32) (nullable) | No |  |
| `createdByUserCode` | string (nullable) | No |  |
| `createdByUserName` | string (nullable) | No |  |

### CreateCoreClaimDtoResponse {#schema-createcoreclaimdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `isResult` | boolean | No |  |
| `result` | string (nullable) | No |  |
| `msg` | string (nullable) | No |  |
| `responseList` | array of [`CreateCoreClaimResponseItem`](#schema-createcoreclaimresponseitem) | No |  |

### CreateCoreClaimDtoResponseServiceResponse {#schema-createcoreclaimdtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`CreateCoreClaimDtoResponse`](#schema-createcoreclaimdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### CreateCoreClaimResponseItem {#schema-createcoreclaimresponseitem}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) (nullable) | No |  |
| `caseRegistrationId` | string (uuid) (nullable) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `casePayableId` | array of string (uuid) | No |  |

### CreateCoreClaimV2DtoRequest {#schema-createcoreclaimv2dtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `requestId` | string (uuid) | Yes |  |
| `claimSourceId` | integer (int32) | No |  |
| `productTypeId` | integer (int32) | No |  |
| `createdByBranchId` | integer (int32) (nullable) | No |  |
| `createdByUserCode` | string (nullable) | No |  |
| `createdByUserName` | string (nullable) | No |  |
| `claims` | array of [`ClaimV2Request`](#schema-claimv2request) | Yes |  |

### CreateRefundRequestDto {#schema-createrefundrequestdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `adjustmentTypeId` | integer (int32) (nullable) | No |  |
| `refundReasonId` | integer (int32) (nullable) | No |  |
| `cacseId` | string (uuid) (nullable) | No |  |
| `claimId` | string (uuid) (nullable) | No |  |
| `refundDate` | string (date-time) (nullable) | No |  |
| `remark` | string (nullable) | No |  |
| `decreaseAmount` | number (double) | No |  |

### CreateRefundResponsetDto {#schema-createrefundresponsetdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseAdjustmentId` | string (uuid) (nullable) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |

### CreateRefundResponsetDtoServiceResponse {#schema-createrefundresponsetdtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`CreateRefundResponsetDto`](#schema-createrefundresponsetdto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### CustomerCheckEligibleCustomerDto {#schema-customercheckeligiblecustomerdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `customerDetailId` | string (uuid) | No |  |
| `cardTypeId` | integer (int32) (nullable) | No |  |
| `cardDetail` | string (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `productTypeId` | integer (int32) (nullable) | No |  |
| `productTypeName` | string (nullable) | No |  |
| `policyCode` | string (nullable) | No |  |
| `customerCode` | string (nullable) | No |  |
| `appStatusId` | integer (int32) (nullable) | No |  |
| `coverageFrom` | string (date-time) (nullable) | No |  |
| `coverageTo` | string (date-time) (nullable) | No |  |
| `productName` | string (nullable) | No |  |
| `schoolName` | string (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |
| `mobilePhoneNumber` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `productCategoryCode` | string (nullable) | No |  |
| `productCategoryName` | string (nullable) | No |  |
| `customerId` | string (uuid) | No |  |
| `titleName` | string (nullable) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `birthDate` | string (date-time) (nullable) | No |  |
| `appStatus` | string (nullable) | No |  |

### CustomerCheckEligibleDtoRequest {#schema-customercheckeligibledtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `type` | string (nullable) | No |  |
| `searchIndex` | integer (int32) | No |  |
| `searchDetail` | string (nullable) | No |  |
| `incidentDate` | string (date-time) (nullable) | No |  |
| `incidentTypeId` | integer (int32) (nullable) | No |  |
| `isContinue` | boolean (nullable) | No |  |

### CustomerCheckEligibleDtoResponse {#schema-customercheckeligibledtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `customer` | [`CustomerCheckEligibleCustomerDto`](#schema-customercheckeligiblecustomerdto) | No |  |
| `benefits` | array of [`GetCustomerBenefitDetailSearchDtoResponse`](#schema-getcustomerbenefitdetailsearchdtoresponse) | No |  |

### CustomerCheckEligibleDtoResponseListServiceResponse {#schema-customercheckeligibledtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`CustomerCheckEligibleDtoResponse`](#schema-customercheckeligibledtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### DisabilityExpenseList {#schema-disabilityexpenselist}
| Field | Type | Required | Description |
|---|---|---|---|
| `benefitId` | integer (int32) (nullable) | No |  |
| `benefitName` | string (nullable) | No |  |
| `originalAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `benefitPercent` | number (double) | No |  |
| `benefitPerUnit` | number (double) | No |  |
| `benefitUnitName` | string (nullable) | No |  |
| `benefitMaxPrice` | number (double) | No |  |

### DocumentReviewOverviewDtoResponse {#schema-documentreviewoverviewdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `totalCount` | integer (int32) | No | จำนวนเอกสารหลังรวมรายการที่มี DocumentId เดียวกันแล้ว |
| `documents` | array of [`DocumentReviewOverviewItemDtoResponse`](#schema-documentreviewoverviewitemdtoresponse) | No | รายการเอกสาร โดยหนึ่ง DocumentId จะแสดงเพียงหนึ่งรายการและมี status เดียว |

### DocumentReviewOverviewItemDtoResponse {#schema-documentreviewoverviewitemdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseId` | string (uuid) | No |  |
| `caseDocumentId` | string (uuid) | No |  |
| `documentId` | string (uuid) (nullable) | No |  |
| `documentNo` | string (nullable) | No |  |
| `documentRemark` | string (nullable) | No |  |
| `documentSubTypeId` | integer (int32) (nullable) | No |  |
| `documentSubTypeCode` | string (nullable) | No |  |
| `documentSubTypeName` | string (nullable) | No |  |
| `documentTypeId` | integer (int32) (nullable) | No |  |
| `documentTypeName` | string (nullable) | No |  |
| `fileCount` | integer (int32) (nullable) | No | จำนวนไฟล์ใต้ DocumentId นี้จาก metadata ของ DocStorage |
| `createdDate` | string (date-time) (nullable) | No |  |
| `updatedDate` | string (date-time) (nullable) | No |  |
| `documentReviewId` | string (uuid) (nullable) | No |  |
| `documentReviewStatusId` | integer (int32) (nullable) | No | status ล่าสุดของเอกสาร โดยรวม review จากทุก CaseDocument ที่ใช้ DocumentId เดียวกัน |
| `documentReviewStatusName` | string (nullable) | No |  |
| `documentReviewRemark` | string (nullable) | No |  |
| `documentReviewByUserId` | integer (int32) (nullable) | No |  |
| `documentReviewDate` | string (date-time) (nullable) | No |  |

### ExpenseCategorySummaryDtoResponse {#schema-expensecategorysummarydtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `approvedAmount` | number (double) | No |  |
| `patientPayAmount` | number (double) | No |  |
| `standardMedicalExpenseCategoryId` | integer (int32) (nullable) | No |  |
| `standardMedicalExpenseCategoryName` | string (nullable) | No |  |

### ExpenseFormatOverviewDtoResponse {#schema-expenseformatoverviewdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `formatTypeId` | integer (int32) | No |  |
| `formatTypeName` | string (nullable) | No |  |

### ExpenseItemOverviewDtoResponse {#schema-expenseitemoverviewdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseItemId` | string (uuid) | No |  |
| `inputToStandardMappingId` | integer (int32) (nullable) | No |  |
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `standardMedicalExpenseName` | string (nullable) | No |  |
| `standardMedicalExpenseCategoryId` | integer (int32) (nullable) | No |  |
| `standardMedicalExpenseCategoryName` | string (nullable) | No |  |
| `nonCoveredReasonId` | integer (int32) (nullable) | No |  |
| `nonCoveredReasonName` | string (nullable) | No |  |
| `quantity` | integer (int32) (nullable) | No |  |
| `perUnit` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `approvedAmount` | number (double) | No |  |
| `patientPayAmount` | number (double) | No |  |
| `remark` | string (nullable) | No |  |

### ExpenseSummaryOverviewDtoResponse {#schema-expensesummaryoverviewdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `formats` | array of [`ExpenseFormatOverviewDtoResponse`](#schema-expenseformatoverviewdtoresponse) | No | รูปแบบรายการค่าใช้จ่ายที่เปิดใช้งานสำหรับหน้า review |
| `items` | array of [`ExpenseItemOverviewDtoResponse`](#schema-expenseitemoverviewdtoresponse) | No | รายการค่าใช้จ่ายของ Case พร้อมยอดก่อนและหลังการพิจารณา |
| `categorySummaries` | array of [`ExpenseCategorySummaryDtoResponse`](#schema-expensecategorysummarydtoresponse) | No | ยอดรวมที่จัดกลุ่มตามหมวดค่าใช้จ่าย |
| `totals` | [`ExpenseTotalsOverviewDtoResponse`](#schema-expensetotalsoverviewdtoresponse) | No |  |

### ExpenseTotalsOverviewDtoResponse {#schema-expensetotalsoverviewdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `approvedAmount` | number (double) | No |  |
| `patientPayAmount` | number (double) | No |  |

### FailedPayTransferTransactionResponseDto {#schema-failedpaytransfertransactionresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `transRefNo` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `payerBankName` | string (nullable) | No |  |
| `statusBank` | string (nullable) | No |  |
| `descriptionTH` | string (nullable) | No |  |
| `status` | string (nullable) | No |  |

### FailedPayTransferTransactionResponseDtoServiceResponse {#schema-failedpaytransfertransactionresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`FailedPayTransferTransactionResponseDto`](#schema-failedpaytransfertransactionresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### FormatTypeDtoResponse {#schema-formattypedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `formatTypeId` | integer (int32) | No | รหัสรูปแบบใบ invoice (Primary Key) |
| `formatTypeName` | string (nullable) | No | ชื่อรูปแบบใบ invoice |
| `isActive` | boolean (nullable) | No | สถานะ |
| `createdByUserId` | integer (int32) (nullable) | No | ผู้สร้าง |
| `createdDate` | string (date-time) (nullable) | No | วันที่สร้าง |
| `updatedByUserId` | integer (int32) (nullable) | No | ผู้แก้ไข |
| `updatedDate` | string (date-time) (nullable) | No | วันที่แก้ไข |

### FormatTypeDtoResponseListServiceResponse {#schema-formattypedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`FormatTypeDtoResponse`](#schema-formattypedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetAdjustmentReasonDtoResponse {#schema-getadjustmentreasondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `adjustmentReasonId` | integer (int32) | No |  |
| `adjustmentReasonName` | string (nullable) | No |  |

### GetAdjustmentReasonDtoResponseListServiceResponse {#schema-getadjustmentreasondtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetAdjustmentReasonDtoResponse`](#schema-getadjustmentreasondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetBankAccountRelationTypeDtoResponse {#schema-getbankaccountrelationtypedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `bankAccountRelationTypeId` | integer (int32) | No |  |
| `bankAccountRelationTypeName` | string (nullable) | No |  |

### GetBankAccountRelationTypeDtoResponseListServiceResponse {#schema-getbankaccountrelationtypedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetBankAccountRelationTypeDtoResponse`](#schema-getbankaccountrelationtypedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetBeneficiaryDtoResponse {#schema-getbeneficiarydtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `policyBeneficiaryId` | string (uuid) | No |  |
| `policyCode` | string (nullable) | No |  |
| `beneficiaryCode` | string (nullable) | No |  |
| `titleId` | integer (int32) (nullable) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `phoneNumber` | string (nullable) | No |  |
| `relationTypeId` | integer (int32) (nullable) | No |  |
| `percentShare` | number (double) (nullable) | No |  |
| `beneficiaryOrder` | integer (int32) (nullable) | No |  |
| `remark` | string (nullable) | No |  |

### GetBeneficiaryDtoResponseListServiceResponse {#schema-getbeneficiarydtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetBeneficiaryDtoResponse`](#schema-getbeneficiarydtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetBenefitDtoResponse {#schema-getbenefitdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `benefitId` | integer (int32) | No |  |
| `benefitName` | string (nullable) | No |  |

### GetBenefitDtoResponseListServiceResponse {#schema-getbenefitdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetBenefitDtoResponse`](#schema-getbenefitdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetBodyPartByDisabilityLossPartDtoResponse {#schema-getbodypartbydisabilitylosspartdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `bodyPartId` | integer (int32) (nullable) | No |  |
| `bodyPartName` | string (nullable) | No |  |
| `disabilitySideId` | integer (int32) (nullable) | No |  |
| `disabilitySideName` | string (nullable) | No |  |
| `disabilityLossSubPartId` | integer (int32) (nullable) | No |  |
| `disabilitySubPartNameTH` | string (nullable) | No |  |
| `lossJointCount` | integer (int32) (nullable) | No |  |
| `disabilitySidePart1Id` | integer (int32) (nullable) | No |  |
| `disabilitySidePart1Name` | string (nullable) | No |  |
| `disabilitySidePart2Id` | integer (int32) (nullable) | No |  |
| `disabilitySidePart2Name` | string (nullable) | No |  |
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |

### GetBodyPartByDisabilityLossPartDtoResponseListServiceResponse {#schema-getbodypartbydisabilitylosspartdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetBodyPartByDisabilityLossPartDtoResponse`](#schema-getbodypartbydisabilitylosspartdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetBranchDtoResponse {#schema-getbranchdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `branchId` | integer (int32) | No |  |
| `branchCode` | string (nullable) | No |  |
| `branchName` | string (nullable) | No |  |

### GetBranchDtoResponseListServiceResponse {#schema-getbranchdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetBranchDtoResponse`](#schema-getbranchdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCancelReasonDtoResponse {#schema-getcancelreasondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `cancelReasonId` | integer (int32) | No |  |
| `cancelReasonName` | string (nullable) | No |  |

### GetCancelReasonDtoResponseListServiceResponse {#schema-getcancelreasondtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetCancelReasonDtoResponse`](#schema-getcancelreasondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCaseByClaimIdDtoResponse {#schema-getcasebyclaimiddtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `incidentDate` | string (date-time) (nullable) | No |  |
| `lastestChiefComplaint` | string (nullable) | No |  |
| `chiefComplaintCustom` | string (nullable) | No |  |
| `totalCaseAmount` | number (double) (nullable) | No |  |
| `totalPaidAmount` | number (double) (nullable) | No |  |
| `caseId` | string (uuid) | No |  |
| `caseNo` | string (nullable) | No |  |
| `occurrenceDate` | string (date-time) (nullable) | No |  |
| `caseChiefComlaint` | string (nullable) | No |  |
| `caseAmount` | number (double) (nullable) | No |  |
| `casePaidAmount` | number (double) (nullable) | No |  |
| `icD10Detail` | string (nullable) | No |  |
| `medicalTypeCode` | string (nullable) | No |  |
| `admissionDate` | string (date-time) (nullable) | No |  |
| `paymentStatusId` | integer (int32) (nullable) | No |  |
| `paymentStatusName` | string (nullable) | No |  |
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `coverageTypeName` | string (nullable) | No |  |
| `incidentTypeName` | string (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### GetCaseByClaimIdDtoResponseListServiceResponse {#schema-getcasebyclaimiddtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetCaseByClaimIdDtoResponse`](#schema-getcasebyclaimiddtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCaseReviewOverviewDtoResponse {#schema-getcasereviewoverviewdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseId` | string (uuid) | No |  |
| `documentReview` | [`DocumentReviewOverviewDtoResponse`](#schema-documentreviewoverviewdtoresponse) | No |  |
| `claimDecision` | [`ClaimDecisionOverviewDtoResponse`](#schema-claimdecisionoverviewdtoresponse) | No |  |
| `expenseSummary` | [`ExpenseSummaryOverviewDtoResponse`](#schema-expensesummaryoverviewdtoresponse) | No |  |

### GetCaseReviewOverviewDtoResponseServiceResponse {#schema-getcasereviewoverviewdtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`GetCaseReviewOverviewDtoResponse`](#schema-getcasereviewoverviewdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetChiefComplaintDtoResponse {#schema-getchiefcomplaintdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `chiefComplaintId` | integer (int32) | No |  |
| `chiefComplaintCode` | string (nullable) | No |  |
| `detail` | string (nullable) | No |  |

### GetChiefComplaintDtoResponseListServiceResponse {#schema-getchiefcomplaintdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetChiefComplaintDtoResponse`](#schema-getchiefcomplaintdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetClaimContinueDtoResponse {#schema-getclaimcontinuedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) | No |  |
| `claimNo` | string (nullable) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `incidentDate` | string (date-time) (nullable) | No |  |
| `chiefComplaintId` | integer (int32) (nullable) | No |  |
| `chiefComplaint` | string (nullable) | No |  |
| `chiefComplaintCustom` | string (nullable) | No |  |
| `admissionDate` | string (date-time) (nullable) | No |  |
| `icD10Detail` | string (nullable) | No |  |
| `totalCaseAmount` | number (double) (nullable) | No |  |
| `totalPaidAmount` | number (double) (nullable) | No |  |
| `claimDetail` | string (nullable) | No |  |
| `remainAmount` | number (double) (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### GetClaimContinueDtoResponseListServiceResponse {#schema-getclaimcontinuedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetClaimContinueDtoResponse`](#schema-getclaimcontinuedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetClaimDetailConsiderDtoResponse {#schema-getclaimdetailconsiderdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) | No |  |
| `notificationDate` | string (date-time) (nullable) | No |  |
| `paymentDate` | string (date-time) (nullable) | No |  |
| `createByUserName` | string (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `productTypeId` | integer (int32) (nullable) | No |  |
| `claimSourceId` | integer (int32) (nullable) | No |  |
| `claimType` | string (nullable) | No |  |
| `claimStatusId` | integer (int32) (nullable) | No |  |
| `claimStatusName` | string (nullable) | No |  |
| `incidentTypeId` | integer (int32) (nullable) | No |  |
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |
| `incidentDate` | string (date-time) (nullable) | No |  |
| `incidentTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `admissionDate` | string (date-time) (nullable) | No |  |
| `admissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `dischargeDate` | string (date-time) (nullable) | No |  |
| `dischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `documentCompleteDate` | string (date-time) (nullable) | No |  |
| `hospitalId` | integer (int32) (nullable) | No |  |
| `chiefComplaintId` | integer (int32) (nullable) | No |  |
| `icD10_1stId` | integer (int32) (nullable) | No |  |
| `icD10_2ndId` | integer (int32) (nullable) | No |  |
| `icD10_3rdId` | integer (int32) (nullable) | No |  |
| `icD10_4thId` | integer (int32) (nullable) | No |  |
| `icD10_5thId` | integer (int32) (nullable) | No |  |
| `icD10_6thId` | integer (int32) (nullable) | No |  |
| `remark` | string (nullable) | No |  |
| `customerDetailId` | string (uuid) (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `claimVersion` | integer (int32) | No |  |
| `claimRowVersion` | string (byte) (nullable) | No |  |
| `caseVersion` | integer (int32) (nullable) | No |  |
| `caseRowVersion` | string (byte) (nullable) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |
| `documentReceivedByUserId` | integer (int32) (nullable) | No |  |
| `documentReceivedByUserName` | string (nullable) | No |  |
| `caseAmount` | number (double) (nullable) | No |  |
| `nplAmount` | number (double) (nullable) | No |  |
| `paymentAmount` | number (double) (nullable) | No |  |
| `hn` | string (nullable) | No |  |
| `an` | string (nullable) | No |  |
| `vn` | string (nullable) | No |  |
| `underlyingDiseaseDetail` | string (nullable) | No |  |
| `treatmentMethod` | string (nullable) | No |  |
| `investigationResults` | string (nullable) | No |  |
| `isProcedurePerformed` | boolean (nullable) | No |  |
| `medicalLicenseNo` | string (nullable) | No |  |
| `physicianName` | string (nullable) | No |  |
| `ipdDayCount` | integer (int32) (nullable) | No |  |
| `icuDayCount` | integer (int32) (nullable) | No |  |
| `admissionIndication` | string (nullable) | No |  |
| `reservationRemark` | string (nullable) | No |  |
| `insuranceCompanyId` | integer (int32) (nullable) | No |  |

### GetClaimDetailConsiderDtoResponseServiceResponse {#schema-getclaimdetailconsiderdtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`GetClaimDetailConsiderDtoResponse`](#schema-getclaimdetailconsiderdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetClaimEditDraftRevisionDtoResponse {#schema-getclaimeditdraftrevisiondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `draftRevisionId` | string (uuid) | No |  |
| `draftId` | string (uuid) | No |  |
| `versionNo` | integer (int32) | No |  |
| `draftStep` | integer (int32) (nullable) | No |  |
| `changedByUserId` | integer (int32) | No |  |
| `changedDate` | string (date-time) | No |  |
| `payload` | [`ClaimEditDraftPayloadDto`](#schema-claimeditdraftpayloaddto) | No |  |

### GetClaimEditDraftRevisionDtoResponseServiceResponse {#schema-getclaimeditdraftrevisiondtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`GetClaimEditDraftRevisionDtoResponse`](#schema-getclaimeditdraftrevisiondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetClaimHistoryDtoResponse {#schema-getclaimhistorydtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `policyCode` | string (nullable) | No |  |
| `claimId` | string (uuid) | No |  |
| `claimNo` | string (nullable) | No |  |
| `incidentDate` | string (date-time) (nullable) | No |  |
| `incidentTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `lastestChiefComplaint` | string (nullable) | No |  |
| `totalCaseAmount` | number (double) (nullable) | No |  |
| `paidAmount` | number (double) (nullable) | No |  |
| `nonCoveredAmount` | number (double) (nullable) | No |  |
| `claimOpenDate` | string (date-time) (nullable) | No |  |
| `countCase` | integer (int32) (nullable) | No |  |
| `icD10Detail` | string (nullable) | No |  |
| `paymentStatusId` | integer (int32) (nullable) | No |  |
| `paymentStatusName` | string (nullable) | No |  |
| `hospitalName` | string (nullable) | No |  |
| `remark` | string (nullable) | No |  |
| `claimType` | string (nullable) | No |  |
| `paymentDate` | string (date-time) (nullable) | No |  |
| `isEnableClaimContinue` | boolean (nullable) | No |  |
| `displayClaimNature` | string (nullable) | No |  |
| `claimStatusId` | integer (int32) (nullable) | No |  |
| `claimStatusName` | string (nullable) | No |  |
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |
| `accidentDescription` | string (nullable) | No |  |
| `icD10Code` | string (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### GetClaimHistoryDtoResponseListServiceResponse {#schema-getclaimhistorydtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetClaimHistoryDtoResponse`](#schema-getclaimhistorydtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetClaimTransactionLogDtoResponse {#schema-getclaimtransactionlogdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `transactionLogId` | string (uuid) | No |  |
| `claimId` | string (uuid) (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `transactionLogTypeId` | integer (int32) (nullable) | No |  |
| `transactionLogTypeName` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `employeeName` | string (nullable) | No |  |
| `totalAmount` | number (double) (nullable) | No |  |
| `paymentStatusId` | integer (int32) (nullable) | No |  |
| `paymentStatusNameTH` | string (nullable) | No |  |
| `decisionId` | integer (int32) (nullable) | No |  |
| `decisionName` | string (nullable) | No |  |
| `transactionLogRemark` | string (nullable) | No |  |
| `referenceId` | string (uuid) (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### GetClaimTransactionLogDtoResponseListServiceResponse {#schema-getclaimtransactionlogdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetClaimTransactionLogDtoResponse`](#schema-getclaimtransactionlogdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetClaimTransactionTypeDtoResponse {#schema-getclaimtransactiontypedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimTransactionTypeId` | integer (int32) | No |  |
| `claimTransactionTypeName` | string (nullable) | No |  |

### GetClaimTransactionTypeDtoResponseListServiceResponse {#schema-getclaimtransactiontypedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetClaimTransactionTypeDtoResponse`](#schema-getclaimtransactiontypedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetContactPersonDtoResponse {#schema-getcontactpersondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `contactPersonTypeId` | integer (int32) (nullable) | No |  |
| `contactPersonTypeName` | string (nullable) | No |  |
| `contactPhoneNo` | string (nullable) | No |  |
| `contactName` | string (nullable) | No |  |
| `indexId` | integer (int32) | No |  |

### GetContactPersonDtoResponseListServiceResponse {#schema-getcontactpersondtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetContactPersonDtoResponse`](#schema-getcontactpersondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetContactPersonTypeDtoResponse {#schema-getcontactpersontypedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `contactPersonTypeId` | integer (int32) | No |  |
| `contactPersonTypeName` | string (nullable) | No |  |

### GetContactPersonTypeDtoResponseListServiceResponse {#schema-getcontactpersontypedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetContactPersonTypeDtoResponse`](#schema-getcontactpersontypedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCustomerBankAccountDtoResponse {#schema-getcustomerbankaccountdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `bankId` | integer (int32) (nullable) | No |  |
| `bankName` | string (nullable) | No |  |
| `bankAccountNo` | string (nullable) | No |  |
| `bankAccountName` | string (nullable) | No |  |
| `bankAccountRelationTypeId` | integer (int32) (nullable) | No |  |
| `bankAccountRelationTypeName` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `indexId` | integer (int32) | No |  |

### GetCustomerBankAccountDtoResponseListServiceResponse {#schema-getcustomerbankaccountdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetCustomerBankAccountDtoResponse`](#schema-getcustomerbankaccountdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCustomerBenefitDetailHalfDtoResponse {#schema-getcustomerbenefitdetailhalfdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `productName` | string (nullable) | No |  |
| `coverageFrom` | string (date-time) (nullable) | No |  |
| `coverageTo` | string (date-time) (nullable) | No |  |
| `benefitName` | string (nullable) | No |  |
| `pricePerUnit` | number (double) (nullable) | No |  |
| `unitName` | string (nullable) | No |  |
| `maxQuantity` | number (double) (nullable) | No |  |
| `quantityUnitName` | string (nullable) | No |  |
| `maxPrice` | number (double) (nullable) | No |  |
| `customerTypeCode` | string (nullable) | No |  |
| `remainBenefit` | number (double) (nullable) | No |  |
| `benefitId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `medicalTypeName` | string (nullable) | No |  |
| `remainAmount` | number (double) (nullable) | No |  |
| `incidentTypeId` | integer (int32) (nullable) | No |  |
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `inputToStandardMappingId` | integer (int32) | No |  |
| `standardMedicalExpenseId` | integer (int32) | No |  |

### GetCustomerBenefitDetailHalfDtoResponseListServiceResponse {#schema-getcustomerbenefitdetailhalfdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetCustomerBenefitDetailHalfDtoResponse`](#schema-getcustomerbenefitdetailhalfdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCustomerBenefitDetailSearchDtoResponse {#schema-getcustomerbenefitdetailsearchdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `productName` | string (nullable) | No |  |
| `coverageFrom` | string (date-time) (nullable) | No |  |
| `coverageTo` | string (date-time) (nullable) | No |  |
| `benefitName` | string (nullable) | No |  |
| `pricePerUnit` | number (double) (nullable) | No |  |
| `unitName` | string (nullable) | No |  |
| `maxQuantity` | number (double) (nullable) | No |  |
| `quantityUnitName` | string (nullable) | No |  |
| `maxPrice` | number (double) (nullable) | No |  |
| `customerTypeCode` | string (nullable) | No |  |
| `remainBenefit` | number (double) (nullable) | No |  |
| `benefitId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `medicalTypeName` | string (nullable) | No |  |
| `remainAmount` | number (double) (nullable) | No |  |

### GetCustomerBenefitDetailSearchDtoResponseListServiceResponse {#schema-getcustomerbenefitdetailsearchdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetCustomerBenefitDetailSearchDtoResponse`](#schema-getcustomerbenefitdetailsearchdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCustomerClaimAdjudicationMonitorDtoResponse {#schema-getcustomerclaimadjudicationmonitordtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) (nullable) | No |  |
| `paymentDate` | string (date-time) (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `schoolName` | string (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `cardDetail` | string (nullable) | No |  |
| `totalAmount` | number (double) (nullable) | No |  |
| `claimTransactionTypeId` | integer (int32) (nullable) | No |  |
| `claimTransactionTypeName` | string (nullable) | No |  |
| `customerDetailId` | string (uuid) (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |
| `caseCount` | integer (int32) (nullable) | No |  |

### GetCustomerClaimAdjudicationMonitorDtoResponseListServiceResponse {#schema-getcustomerclaimadjudicationmonitordtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetCustomerClaimAdjudicationMonitorDtoResponse`](#schema-getcustomerclaimadjudicationmonitordtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCustomerDetailByIdDtoResponse {#schema-getcustomerdetailbyiddtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `customerDetailId` | string (uuid) | No |  |
| `customerId` | string (uuid) | No |  |
| `policyCode` | string (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `customerTypeCode` | string (nullable) | No |  |
| `customerTypeName` | string (nullable) | No |  |
| `cardTypeId` | integer (int32) (nullable) | No |  |
| `cardDetail` | string (nullable) | No |  |
| `productTypeId` | integer (int32) (nullable) | No |  |
| `certificateNo` | string (nullable) | No |  |
| `policyNo` | string (nullable) | No |  |
| `productTypeName` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `productName` | string (nullable) | No |  |
| `coverageFrom` | string (date-time) (nullable) | No |  |
| `coverageTo` | string (date-time) (nullable) | No |  |
| `customerCode` | string (nullable) | No |  |
| `schoolName` | string (nullable) | No |  |
| `provinceName` | string (nullable) | No |  |
| `districtName` | string (nullable) | No |  |
| `subDistrictName` | string (nullable) | No |  |
| `address` | string (nullable) | No |  |
| `mobilePhoneNumber` | string (nullable) | No |  |
| `birthDate` | string (date-time) (nullable) | No |  |
| `genderName` | string (nullable) | No |  |
| `occupationName` | string (nullable) | No |  |
| `titleName` | string (nullable) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `insuredCompanyId` | integer (int32) (nullable) | No |  |
| `productCategoryCode` | string (nullable) | No |  |
| `productCategoryName` | string (nullable) | No |  |
| `contactName` | string (nullable) | No |  |
| `contactPhoneNo` | string (nullable) | No |  |
| `contactPositionId` | integer (int32) (nullable) | No |  |
| `contactPositionName` | string (nullable) | No |  |
| `policyExcludeId` | integer (int32) (nullable) | No |  |
| `policyExcludeDetail` | string (nullable) | No |  |
| `agentName` | string (nullable) | No |  |
| `agentBranchName` | string (nullable) | No |  |
| `levelRoomName` | string (nullable) | No |  |
| `academicYear` | integer (int32) (nullable) | No |  |
| `appStatusId` | integer (int32) (nullable) | No |  |
| `appStatus` | string (nullable) | No |  |
| `bankId` | integer (int32) (nullable) | No |  |
| `bankAccountNo` | string (nullable) | No |  |
| `bankAccountName` | string (nullable) | No |  |
| `customerPaymentStatusCode` | string (nullable) | No |  |
| `customerPaymentStatus` | string (nullable) | No |  |

### GetCustomerDetailByIdDtoResponseServiceResponse {#schema-getcustomerdetailbyiddtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`GetCustomerDetailByIdDtoResponse`](#schema-getcustomerdetailbyiddtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCustomerSearchByPolicyCodeDtoResponse {#schema-getcustomersearchbypolicycodedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `customerDetailId` | string (uuid) | No |  |
| `cardTypeId` | integer (int32) (nullable) | No |  |
| `cardDetail` | string (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `productTypeId` | integer (int32) (nullable) | No |  |
| `productTypeName` | string (nullable) | No |  |
| `policyCode` | string (nullable) | No |  |
| `customerCode` | string (nullable) | No |  |
| `appStatusId` | integer (int32) (nullable) | No |  |
| `coverageFrom` | string (date-time) (nullable) | No |  |
| `coverageTo` | string (date-time) (nullable) | No |  |
| `productName` | string (nullable) | No |  |
| `schoolName` | string (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |
| `mobilePhoneNumber` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `productCategoryCode` | string (nullable) | No |  |
| `productCategoryName` | string (nullable) | No |  |
| `memberNo` | string (nullable) | No |  |

### GetCustomerSearchByPolicyCodeDtoResponseListServiceResponse {#schema-getcustomersearchbypolicycodedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetCustomerSearchByPolicyCodeDtoResponse`](#schema-getcustomersearchbypolicycodedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetCustomerSearchDtoResponse {#schema-getcustomersearchdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `customerDetailId` | string (uuid) | No |  |
| `cardTypeId` | integer (int32) (nullable) | No |  |
| `cardDetail` | string (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `productTypeId` | integer (int32) (nullable) | No |  |
| `productTypeName` | string (nullable) | No |  |
| `policyCode` | string (nullable) | No |  |
| `customerCode` | string (nullable) | No |  |
| `appStatusId` | integer (int32) (nullable) | No |  |
| `coverageFrom` | string (date-time) (nullable) | No |  |
| `coverageTo` | string (date-time) (nullable) | No |  |
| `productName` | string (nullable) | No |  |
| `schoolName` | string (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |
| `mobilePhoneNumber` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `productCategoryCode` | string (nullable) | No |  |
| `productCategoryName` | string (nullable) | No |  |

### GetCustomerSearchDtoResponseListServiceResponse {#schema-getcustomersearchdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetCustomerSearchDtoResponse`](#schema-getcustomersearchdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDashboardCustomerConsiderDtoResponse {#schema-getdashboardcustomerconsiderdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `customerTotalCount` | integer (int32) (nullable) | No |  |
| `customerWaitReceiveCount` | integer (int32) (nullable) | No |  |
| `customerWaitPaymentCount` | integer (int32) (nullable) | No |  |
| `customerCancelCount` | integer (int32) (nullable) | No |  |
| `hospitalTotalCount` | integer (int32) (nullable) | No |  |
| `hospitalCheckEligibilityCount` | integer (int32) (nullable) | No |  |
| `hospitalWaitConsiderCount` | integer (int32) (nullable) | No |  |
| `hospitalRequestBillingCount` | integer (int32) (nullable) | No |  |

### GetDashboardCustomerConsiderDtoResponseListServiceResponse {#schema-getdashboardcustomerconsiderdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDashboardCustomerConsiderDtoResponse`](#schema-getdashboardcustomerconsiderdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDCRDtoResponse {#schema-getdcrdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `policyCode` | string (nullable) | No |  |
| `period` | string (date-time) | No |  |
| `insuredCompanyName` | string (nullable) | No |  |
| `productName` | string (nullable) | No |  |
| `premiumDept` | number (double) (nullable) | No |  |
| `premiumRecieve` | number (double) (nullable) | No |  |
| `paymentTypeId` | integer (int32) (nullable) | No |  |
| `payMethodCode` | string (nullable) | No |  |
| `paymentType` | string (nullable) | No |  |
| `policyNo` | string (nullable) | No |  |
| `bankTransactionDatetime` | string (date-time) (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### GetDCRDtoResponseListServiceResponse {#schema-getdcrdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDCRDtoResponse`](#schema-getdcrdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDecisionDtoResponse {#schema-getdecisiondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `decisionId` | integer (int32) | No |  |
| `decisionNameTH` | string (nullable) | No |  |

### GetDecisionDtoResponseListServiceResponse {#schema-getdecisiondtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDecisionDtoResponse`](#schema-getdecisiondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDecisionReasonDtoResponse {#schema-getdecisionreasondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `decisionReasonId` | integer (int32) | No |  |
| `decisionReasonName` | string (nullable) | No |  |

### GetDecisionReasonDtoResponseListServiceResponse {#schema-getdecisionreasondtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDecisionReasonDtoResponse`](#schema-getdecisionreasondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDecreaseTransactionRefundResponseDto {#schema-getdecreasetransactionrefundresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `transactionDate` | string (date-time) (nullable) | No | วันที่ทำรายการ |
| `transactionTypeName` | string (nullable) | No | ประเภทการทำรายการ |
| `decreaseAmount` | number (double) (nullable) | No | ยอดลดจ่าย |
| `description` | string (nullable) | No | รายละเอียดการทำรายการ |

### GetDecreaseTransactionRefundResponseDtoListServiceResponse {#schema-getdecreasetransactionrefundresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDecreaseTransactionRefundResponseDto`](#schema-getdecreasetransactionrefundresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDecreaseTransactionResponseDto {#schema-getdecreasetransactionresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `transactionDate` | string (date-time) (nullable) | No |  |
| `transactionTypeName` | string (nullable) | No |  |
| `decreaseAmount` | number (double) (nullable) | No |  |
| `description` | string (nullable) | No |  |

### GetDecreaseTransactionResponseDtoListServiceResponse {#schema-getdecreasetransactionresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDecreaseTransactionResponseDto`](#schema-getdecreasetransactionresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDeductionSourceDtoResponse {#schema-getdeductionsourcedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `deductionSourceId` | integer (int32) | No |  |
| `deductionSourceName` | string (nullable) | No |  |

### GetDeductionSourceDtoResponseListServiceResponse {#schema-getdeductionsourcedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDeductionSourceDtoResponse`](#schema-getdeductionsourcedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDisabilityLossPartDtoResponse {#schema-getdisabilitylosspartdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `disabilityLossPartId` | integer (int32) | No |  |
| `disabilityLossPartCode` | string (nullable) | No |  |
| `disabilityLossPartNameTH` | string (nullable) | No |  |
| `disabilityLossPartNameEN` | string (nullable) | No |  |

### GetDisabilityLossPartDtoResponseListServiceResponse {#schema-getdisabilitylosspartdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDisabilityLossPartDtoResponse`](#schema-getdisabilitylosspartdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDocumentByCaseIdDtoResponse {#schema-getdocumentbycaseiddtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDocumentId` | string (uuid) | No |  |
| `documentId` | string (uuid) (nullable) | No |  |
| `documentCode` | string (nullable) | No |  |
| `documentSubTypeId` | integer (int32) (nullable) | No |  |
| `claimDocumentTypeName` | string (nullable) | No |  |
| `claimDocumentTypeId` | integer (int32) (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### GetDocumentByCaseIdDtoResponseListServiceResponse {#schema-getdocumentbycaseiddtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDocumentByCaseIdDtoResponse`](#schema-getdocumentbycaseiddtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDocumentRecipientTypeDtoResponse {#schema-getdocumentrecipienttypedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `documentRecipientTypeId` | integer (int32) | No |  |
| `documentRecipientTypeName` | string (nullable) | No |  |

### GetDocumentRecipientTypeDtoResponseListServiceResponse {#schema-getdocumentrecipienttypedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDocumentRecipientTypeDtoResponse`](#schema-getdocumentrecipienttypedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDocumentReviewStatusDtoResponse {#schema-getdocumentreviewstatusdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `documentReviewStatusId` | integer (int32) | No |  |
| `documentReviewStatusName` | string (nullable) | No |  |
| `indexId` | integer (int32) | No |  |

### GetDocumentReviewStatusDtoResponseListServiceResponse {#schema-getdocumentreviewstatusdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDocumentReviewStatusDtoResponse`](#schema-getdocumentreviewstatusdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetDocumentSubTypeDtoRequest {#schema-getdocumentsubtypedtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `documentTypeId` | integer (int32) | Yes |  |
| `documentPrefix` | string | Yes |  |
| `productTypeId` | integer (int32) | Yes |  |

### GetDocumentSubTypeDtoResponse {#schema-getdocumentsubtypedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `documentId` | string (uuid) (nullable) | No |  |
| `documentCode` | string (nullable) | No |  |
| `documentSubTypeId` | integer (int32) (nullable) | No |  |
| `documentSubTypeName` | string (nullable) | No |  |
| `documentTypeId` | integer (int32) (nullable) | No |  |

### GetDocumentSubTypeDtoResponseListServiceResponse {#schema-getdocumentsubtypedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetDocumentSubTypeDtoResponse`](#schema-getdocumentsubtypedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetEmployeeClaimPaymentLimitResponse {#schema-getemployeeclaimpaymentlimitresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `employeeClaimPaymentLimitId` | string (uuid) | No |  |
| `userId` | integer (int32) | No |  |
| `paymentLimit` | number (double) | No |  |
| `usedAmount` | number (double) | No |  |
| `remainingAmount` | number (double) | No |  |
| `requestedTransferAmount` | number (double) | No |  |
| `remainingAfterRequestAmount` | number (double) | No |  |
| `isLimitSufficient` | boolean | No |  |
| `validationMessage` | string (nullable) | No |  |

### GetEmployeeClaimPaymentLimitResponseServiceResponse {#schema-getemployeeclaimpaymentlimitresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`GetEmployeeClaimPaymentLimitResponse`](#schema-getemployeeclaimpaymentlimitresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetHospitalClaimAdjudicationMonitorDtoResponse {#schema-gethospitalclaimadjudicationmonitordtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) (nullable) | No |  |
| `decisionDate` | string (date-time) (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `hospitalName` | string (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `medicalType` | string (nullable) | No |  |
| `productName` | string (nullable) | No |  |
| `totalAmount` | number (double) (nullable) | No |  |
| `claimTransactionTypeId` | integer (int32) (nullable) | No |  |
| `claimTransactionTypeName` | string (nullable) | No |  |
| `cardDetail` | string (nullable) | No |  |
| `customerDetailId` | string (uuid) (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |

### GetHospitalClaimAdjudicationMonitorDtoResponseListServiceResponse {#schema-gethospitalclaimadjudicationmonitordtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetHospitalClaimAdjudicationMonitorDtoResponse`](#schema-gethospitalclaimadjudicationmonitordtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetICD10DtoResponse {#schema-geticd10dtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `icD10Id` | integer (int32) | No |  |
| `icD10Code` | string (nullable) | No |  |
| `icD10Detail` | string (nullable) | No |  |

### GetICD10DtoResponseListServiceResponse {#schema-geticd10dtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetICD10DtoResponse`](#schema-geticd10dtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetIncidentTypeMappingDtoResponse {#schema-getincidenttypemappingdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `incidentTypeId` | integer (int32) (nullable) | No |  |
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `coverageTypeNameTH` | string (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `medicalTypeCode` | string (nullable) | No |  |
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |
| `causeOfIncidentName` | string (nullable) | No |  |
| `indexId` | integer (int32) | No |  |

### GetIncidentTypeMappingDtoResponseListServiceResponse {#schema-getincidenttypemappingdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetIncidentTypeMappingDtoResponse`](#schema-getincidenttypemappingdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetIncreaseTransferLimitDetailResponseDto {#schema-getincreasetransferlimitdetailresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseTransferApprovalId` | string (uuid) | No |  |
| `claimNo` | string (nullable) | No |  |
| `requestedTransferAmount` | number (double) (nullable) | No |  |
| `paymentLimitAmount` | number (double) (nullable) | No |  |
| `excessAmount` | number (double) (nullable) | No |  |
| `remainingLimitAmount` | number (double) (nullable) | No |  |
| `customerName` | string (nullable) | No |  |

### GetIncreaseTransferLimitDetailResponseDtoServiceResponse {#schema-getincreasetransferlimitdetailresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`GetIncreaseTransferLimitDetailResponseDto`](#schema-getincreasetransferlimitdetailresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetInsuranceCompanyDtoResponse {#schema-getinsurancecompanydtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `organizeId` | integer (int32) | No |  |
| `organizeCode` | string (nullable) | No |  |
| `organizeName` | string (nullable) | No |  |
| `organizeTypeId` | integer (int32) (nullable) | No |  |
| `shortName` | string (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### GetInsuranceCompanyDtoResponseListServiceResponse {#schema-getinsurancecompanydtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetInsuranceCompanyDtoResponse`](#schema-getinsurancecompanydtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetNonCoveredReasonDtoResponse {#schema-getnoncoveredreasondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `nonCoveredReasonId` | integer (int32) | No |  |
| `nonCoveredReasonName` | string (nullable) | No |  |

### GetNonCoveredReasonDtoResponseListServiceResponse {#schema-getnoncoveredreasondtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetNonCoveredReasonDtoResponse`](#schema-getnoncoveredreasondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetOrganizeDtoResponse {#schema-getorganizedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `organizeId` | integer (int32) | No |  |
| `organizeName` | string (nullable) | No |  |

### GetOrganizeDtoResponseListServiceResponse {#schema-getorganizedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetOrganizeDtoResponse`](#schema-getorganizedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetPaymentStatusDtoResponse {#schema-getpaymentstatusdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `paymentStatusId` | integer (int32) | No |  |
| `paymentStatusNameTH` | string (nullable) | No |  |

### GetPaymentStatusDtoResponseListServiceResponse {#schema-getpaymentstatusdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetPaymentStatusDtoResponse`](#schema-getpaymentstatusdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetPolicyBenefitDtoResponse {#schema-getpolicybenefitdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `policyCode` | string (nullable) | No |  |
| `benefitId` | integer (int32) (nullable) | No |  |
| `benefitTypeName` | string (nullable) | No |  |
| `benefitName` | string (nullable) | No |  |
| `pricePerUnit` | number (double) (nullable) | No |  |
| `pricePerUnitName` | string (nullable) | No |  |
| `maxPrice` | number (double) (nullable) | No |  |
| `maxQuantity` | integer (int32) (nullable) | No |  |
| `quantityUnitName` | string (nullable) | No |  |
| `customerTypeCode` | string (nullable) | No |  |
| `fullBenefitDisplay` | string (nullable) | No |  |

### GetPolicyBenefitDtoResponseListServiceResponse {#schema-getpolicybenefitdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetPolicyBenefitDtoResponse`](#schema-getpolicybenefitdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetPolicyBenefitSheredDtoResponse {#schema-getpolicybenefitshereddtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `policyCode` | string (nullable) | No |  |
| `benefitId` | integer (int32) | No |  |
| `benefitCode` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `benefitName` | string (nullable) | No |  |
| `maxPrice` | number (double) (nullable) | No |  |
| `customerTypeCode` | string (nullable) | No |  |
| `shortBenefit` | string (nullable) | No |  |
| `fullBenefitDisplay` | string (nullable) | No |  |

### GetPolicyBenefitSheredDtoResponseListServiceResponse {#schema-getpolicybenefitshereddtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetPolicyBenefitSheredDtoResponse`](#schema-getpolicybenefitshereddtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetPreviousClaimDtoResponse {#schema-getpreviousclaimdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `incidentTypeId` | integer (int32) (nullable) | No |  |
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `incidentDate` | string (date-time) (nullable) | No |  |
| `deathDate` | string (date-time) (nullable) | No |  |
| `placeOfDeathId` | integer (int32) (nullable) | No |  |
| `placeOfDeathDetail` | string (nullable) | No |  |
| `icD10Id` | integer (int32) (nullable) | No |  |
| `icD10DescriptionTH` | string (nullable) | No |  |
| `paymentStatusId` | integer (int32) (nullable) | No |  |
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |
| `totalCaseAmount` | number (double) | No |  |
| `totalNetPaidAmount` | number (double) | No |  |
| `remainingCoverageLimit` | number (double) | No |  |
| `remainingAmountAfterPreviousClaim` | number (double) (nullable) | No |  |
| `chiefComplaintCustom` | string (nullable) | No |  |
| `chiefComplaintId` | integer (int32) (nullable) | No |  |
| `hospitalId` | integer (int32) (nullable) | No |  |

### GetPreviousClaimDtoResponseServiceResponse {#schema-getpreviousclaimdtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`GetPreviousClaimDtoResponse`](#schema-getpreviousclaimdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetProvinceDtoResponse {#schema-getprovincedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `provinceId` | integer (int32) | No |  |
| `provinceName` | string (nullable) | No |  |

### GetProvinceDtoResponseListServiceResponse {#schema-getprovincedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetProvinceDtoResponse`](#schema-getprovincedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetRejectReasonDtoResponse {#schema-getrejectreasondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `rejectReasonId` | integer (int32) | No |  |
| `rejectReasonName` | string (nullable) | No |  |

### GetRejectReasonDtoResponseListServiceResponse {#schema-getrejectreasondtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetRejectReasonDtoResponse`](#schema-getrejectreasondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetRelationTypeDtoResponse {#schema-getrelationtypedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `relationTypeId` | integer (int32) | No |  |
| `relationTypeCode` | string (nullable) | No |  |
| `relationTypeName` | string (nullable) | No |  |

### GetRelationTypeDtoResponseListServiceResponse {#schema-getrelationtypedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetRelationTypeDtoResponse`](#schema-getrelationtypedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetStandardMedicalExpenseByCaseDtoResponse {#schema-getstandardmedicalexpensebycasedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `inputToStandardMappingId` | integer (int32) (nullable) | No |  |
| `formatTypeId` | integer (int32) (nullable) | No |  |
| `inputItemCode` | string (nullable) | No |  |
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `descriptionEN` | string (nullable) | No |  |
| `descriptionTH` | string (nullable) | No |  |
| `maximumLimit` | number (double) (nullable) | No |  |
| `standardMedicalExpenseCategoryId` | integer (int32) (nullable) | No |  |
| `backgroundColorCode` | string (nullable) | No |  |
| `inputToStandardSubCategoryId` | integer (int32) (nullable) | No |  |
| `bodyPartId` | integer (int32) (nullable) | No |  |
| `caseItemId` | string (uuid) (nullable) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |
| `quantity` | integer (int32) (nullable) | No |  |
| `perUnit` | integer (int32) (nullable) | No |  |
| `originalAmount` | number (double) (nullable) | No |  |
| `discountAmount` | number (double) (nullable) | No |  |
| `netCaseAmount` | number (double) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nonCoveredAmount` | number (double) (nullable) | No |  |
| `nonCoveredReasonId` | integer (int32) (nullable) | No |  |
| `remark` | string (nullable) | No |  |
| `caseAdjudicationId` | string (uuid) (nullable) | No |  |
| `benefitId` | integer (int32) (nullable) | No |  |

### GetStandardMedicalExpenseByCaseDtoResponseListServiceResponse {#schema-getstandardmedicalexpensebycasedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetStandardMedicalExpenseByCaseDtoResponse`](#schema-getstandardmedicalexpensebycasedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetTitleDtoResponse {#schema-gettitledtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `titleId` | integer (int32) | No |  |
| `titleName` | string (nullable) | No |  |

### GetTitleDtoResponseListServiceResponse {#schema-gettitledtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetTitleDtoResponse`](#schema-gettitledtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### GetZebraCarOwnerDtoResponse {#schema-getzebracarownerdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `zebraId` | integer (int32) | No |  |
| `zebraCode` | string (nullable) | No |  |
| `zebraNo` | string (nullable) | No |  |
| `employeeId` | integer (int32) | No |  |
| `employeeCode` | string (nullable) | No |  |
| `employeeName` | string (nullable) | No |  |
| `employeeNickName` | string (nullable) | No |  |
| `employeeFullName` | string (nullable) | No |  |

### GetZebraCarOwnerDtoResponseListServiceResponse {#schema-getzebracarownerdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`GetZebraCarOwnerDtoResponse`](#schema-getzebracarownerdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### IncidentTypeDtoResponse {#schema-incidenttypedtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `indexId` | integer (int32) | No |  |
| `incidentTypeId` | integer (int32) | No |  |
| `incidentTypeNameTH` | string (nullable) | No |  |

### IncidentTypeDtoResponseListServiceResponse {#schema-incidenttypedtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`IncidentTypeDtoResponse`](#schema-incidenttypedtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### IncreaseTransferLimitChangeStatusRequestDto {#schema-increasetransferlimitchangestatusrequestdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseTransferApprovalId` | string (uuid) | No |  |
| `transferApprovalStatusId` | integer (int32) | Yes | สาะนะรายการ |
| `approvalRemark` | string (nullable) | No | หมายเหตุการอนุมัติ ส่วนเหตุผลปฏิเสธเก็บใน TransactionLog |

### IncreaseTransferLimitChangeStatusResponseDto {#schema-increasetransferlimitchangestatusresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |

### IncreaseTransferLimitChangeStatusResponseDtoServiceResponse {#schema-increasetransferlimitchangestatusresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`IncreaseTransferLimitChangeStatusResponseDto`](#schema-increasetransferlimitchangestatusresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### IncreaseTransferLimitMonitorResponseDto {#schema-increasetransferlimitmonitorresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseId` | string (uuid) | No |  |
| `caseNo` | string (nullable) | No | เลขที่ CC |
| `claimNo` | string (nullable) | No | เลขที่ CL |
| `createdDate` | string (date-time) (nullable) | No | วันที่สร้างเคลม |
| `branchName` | string (nullable) | No | สาขา |
| `amount` | number (double) (nullable) | No | จำนวนเงิน |
| `toAccountNo` | string (nullable) | No | เลขที่บัญชี |
| `transferType` | string (nullable) | No | ประเภทโอนเงิน |
| `remark` | string (nullable) | No | สาเหตุ |
| `transferApprovalStatusName` | string (nullable) | No |  |
| `transferApprovalStatusId` | integer (int32) | No |  |

### IncreaseTransferLimitMonitorResponseDtoListServiceResponse {#schema-increasetransferlimitmonitorresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`IncreaseTransferLimitMonitorResponseDto`](#schema-increasetransferlimitmonitorresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### InputToStandardMappingDtoResponse {#schema-inputtostandardmappingdtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `inputToStandardMappingId` | integer (int32) | No |  |
| `formatTypeId` | integer (int32) (nullable) | No |  |
| `inputItemCode` | string (nullable) | No |  |
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `descriptionEN` | string (nullable) | No |  |
| `descriptionTH` | string (nullable) | No |  |
| `maximumLimit` | number (double) | No |  |
| `standardMedicalExpenseCategoryId` | integer (int32) (nullable) | No |  |
| `backgroundColorCode` | string (nullable) | No |  |
| `inputToStandardSubCategoryId` | integer (int32) (nullable) | No |  |
| `bodyPartId` | integer (int32) (nullable) | No |  |

### InputToStandardMappingDtoResponseListServiceResponse {#schema-inputtostandardmappingdtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`InputToStandardMappingDtoResponse`](#schema-inputtostandardmappingdtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### InputToStandardSubCategoryList {#schema-inputtostandardsubcategorylist}
| Field | Type | Required | Description |
|---|---|---|---|
| `inputToStandardSubCategoryId` | integer (int32) | No |  |
| `inputToStandardSubCategoryName` | string (nullable) | No |  |
| `inputToStandardMappingList` | array of [`InputToStandardMappingDtoResponse`](#schema-inputtostandardmappingdtoresponse) | No |  |

### MedicalExpenseList {#schema-medicalexpenselist}
| Field | Type | Required | Description |
|---|---|---|---|
| `benefitId` | integer (int32) (nullable) | No |  |
| `benefitName` | string (nullable) | No |  |
| `net` | number (double) | No |  |
| `cover` | number (double) | No |  |
| `unCover` | number (double) | No |  |
| `pay` | number (double) | No |  |
| `unPay` | number (double) | No |  |

### PaymentStatusResponseDto {#schema-paymentstatusresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | integer (int32) | No |  |
| `name` | string (nullable) | No |  |

### PaymentStatusResponseDtoListServiceResponse {#schema-paymentstatusresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`PaymentStatusResponseDto`](#schema-paymentstatusresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### PayTransferSettingHistoryResponseDto {#schema-paytransfersettinghistoryresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `createdByUser` | string (nullable) | No |  |
| `employeeCode` | string (nullable) | No |  |
| `isAutoTransfer` | boolean | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |

### PayTransferSettingResponseDto {#schema-paytransfersettingresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `paytransferSettingId` | string (uuid) | No |  |
| `isAutoTransfer` | boolean | No |  |
| `history` | array of [`PayTransferSettingHistoryResponseDto`](#schema-paytransfersettinghistoryresponsedto) | No |  |

### PayTransferSettingResponseDtoServiceResponse {#schema-paytransfersettingresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`PayTransferSettingResponseDto`](#schema-paytransfersettingresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### ProblemDetails {#schema-problemdetails}
| Field | Type | Required | Description |
|---|---|---|---|
| `type` | string (nullable) | No |  |
| `title` | string (nullable) | No |  |
| `status` | integer (int32) (nullable) | No |  |
| `detail` | string (nullable) | No |  |
| `instance` | string (nullable) | No |  |

### RefundApproveMonitorRequestDto {#schema-refundapprovemonitorrequestdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `branceId` | integer (int32) (nullable) | No |  |
| `refundStatusId` | integer (int32) (nullable) | No |  |
| `fromDate` | string (date-time) (nullable) | No |  |
| `toDate` | string (date-time) (nullable) | No |  |

### RefundApproveMonitorResponse {#schema-refundapprovemonitorresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseId` | string (uuid) (nullable) | No |  |
| `claimId` | string (uuid) (nullable) | No |  |
| `refundNo` | string (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `remark` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No |  |
| `refundAmount` | number (double) (nullable) | No |  |
| `refundStatusId` | integer (int32) (nullable) | No |  |
| `refundStatusNameTH` | string (nullable) | No |  |
| `branceName` | string (nullable) | No |  |
| `caseRefundId` | string (uuid) | No |  |

### RefundApproveMonitorResponseListServiceResponse {#schema-refundapprovemonitorresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`RefundApproveMonitorResponse`](#schema-refundapprovemonitorresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### RefundDetailsAccountDetailsResponseDto {#schema-refunddetailsaccountdetailsresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `toAccountName` | string (nullable) | No | ชื่อบัญชีปลายทางผู้รับเงิน |
| `toAccountNo` | string (nullable) | No | เลขที่บัญชีปลายทางผู้รับเงิน |
| `toBank` | string (nullable) | No | ชื่อธนาคารปลายทางผู้รับเงิน |
| `toBankId` | integer (int32) (nullable) | No | รหัสธนาคารปลายทางผู้รับเงิน |
| `phoneNo` | string (nullable) | No | หมายเลขโทรศัพท์สำหรับส่งข้อความ |
| `casePayableId` | string (uuid) | No | รหัสยอดค้างจ่าย/เจ้าหนี้ของเคลม |
| `claimantAccountRelationship` | string (nullable) | No | ความสัมพันธ์ของบัญชีกับผู้รับสินไหม |

### RefundDetailsAccountDetailsResponseDtoListServiceResponse {#schema-refunddetailsaccountdetailsresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`RefundDetailsAccountDetailsResponseDto`](#schema-refunddetailsaccountdetailsresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### RefundMonitorRequestDto {#schema-refundmonitorrequestdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `branceId` | integer (int32) (nullable) | No |  |
| `refundStatusId` | integer (int32) (nullable) | No |  |

### RefundMonitorResponse {#schema-refundmonitorresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseId` | string (uuid) (nullable) | No |  |
| `claimId` | string (uuid) (nullable) | No |  |
| `refundNo` | string (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `remark` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No |  |
| `refundAmount` | number (double) (nullable) | No |  |
| `refundStatusId` | integer (int32) (nullable) | No |  |
| `refundStatusNameTH` | string (nullable) | No |  |

### RefundMonitorResponseListServiceResponse {#schema-refundmonitorresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`RefundMonitorResponse`](#schema-refundmonitorresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### RefundReasonResponseDto {#schema-refundreasonresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | integer (int32) | No |  |
| `name` | string (nullable) | No |  |

### RefundReasonResponseDtoListServiceResponse {#schema-refundreasonresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`RefundReasonResponseDto`](#schema-refundreasonresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### RefundReasonResponseDtoServiceResponse {#schema-refundreasonresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`RefundReasonResponseDto`](#schema-refundreasonresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### RefundStatusResponseDto {#schema-refundstatusresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `id` | integer (int32) | No |  |
| `name` | string (nullable) | No |  |

### RefundStatusResponseDtoListServiceResponse {#schema-refundstatusresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`RefundStatusResponseDto`](#schema-refundstatusresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### RefundTransactionResponseDto {#schema-refundtransactionresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `remark` | string (nullable) | No | หมายเหตุการทำรายการ |
| `transactionDate` | string (date-time) (nullable) | No | วันที่ทำรายการ |
| `claimTransactionTypeName` | string (nullable) | No | ประเภทการทำรายการ |
| `createdByFullName` | string (nullable) | No | ผู้ทำรายการ |
| `amountTotal` | number (double) (nullable) | No | จำนวนเงินรวม |

### RefundTransactionResponseDtoListServiceResponse {#schema-refundtransactionresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`RefundTransactionResponseDto`](#schema-refundtransactionresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### RefundTransferTransactionDetailResponseDto {#schema-refundtransfertransactiondetailresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `paymentCode` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `paymentTypeName` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No |  |
| `toBankName` | string (nullable) | No |  |
| `toBankAccountNo` | string (nullable) | No |  |
| `toBankAccountName` | string (nullable) | No |  |

### RefundTransferTransactionResponseDto {#schema-refundtransfertransactionresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `payTransferDetails` | array of [`RefundTransferTransactionDetailResponseDto`](#schema-refundtransfertransactiondetailresponsedto) | No |  |

### RefundTransferTransactionResponseDtoServiceResponse {#schema-refundtransfertransactionresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`RefundTransferTransactionResponseDto`](#schema-refundtransfertransactionresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### SaveAdditionalTransferRequest {#schema-saveadditionaltransferrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseId` | string (uuid) | No | รหัสเคลม |
| `claimNo` | string (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) | No | ยอดเงินที่โอนเพิ่ |
| `toBankId` | integer (int32) | Yes | รหัสธนาคารปลายทาง |
| `toBankName` | string | Yes | ชื่อธนาคารปลายทาง |
| `toBankAccountNo` | string | Yes | เลขที่บัญชีปลายทาง |
| `toBankAccountName` | string | Yes | ชื่อบัญชีปลายทาง |
| `phoneNumber` | string | Yes | หมายเลขโทรศัพท์ |
| `adjustmentReasonId` | integer (int32) (nullable) | No |  |
| `remark` | string (nullable) | No |  |

### SaveAdditionalTransferResponseDto {#schema-saveadditionaltransferresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `casePayableId` | string (uuid) (nullable) | No |  |

### SaveAdditionalTransferResponseDtoServiceResponse {#schema-saveadditionaltransferresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`SaveAdditionalTransferResponseDto`](#schema-saveadditionaltransferresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### SaveClaimEditDraftDtoRequest {#schema-saveclaimeditdraftdtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) | No |  |
| `caseId` | string (uuid) | No |  |
| `incidentTypeId` | integer (int32) | No |  |
| `incidentDate` | string (date-time) | No |  |
| `incidentTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `accidentPlace` | string (nullable) | No |  |
| `accidentDescription` | string (nullable) | No |  |
| `draftStep` | integer (int32) | No |  |
| `case` | [`CaseSaveClaimEditDraftRequest`](#schema-casesaveclaimeditdraftrequest) | No |  |
| `claimEditDraft` | [`ClaimEditDraftSaveClaimEditDraftRequest`](#schema-claimeditdraftsaveclaimeditdraftrequest) | No |  |

### SaveClaimEditDraftDtoRespone {#schema-saveclaimeditdraftdtorespone}
| Field | Type | Required | Description |
|---|---|---|---|
| `isResult` | boolean | No |  |
| `result` | string (nullable) | No |  |
| `msg` | string (nullable) | No |  |

### SaveClaimEditDraftDtoResponeServiceResponse {#schema-saveclaimeditdraftdtoresponeserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`SaveClaimEditDraftDtoRespone`](#schema-saveclaimeditdraftdtorespone) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### SaveRefundDetailsResponseDto {#schema-saverefunddetailsresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) | No |  |
| `caseId` | string (uuid) | No |  |
| `claimNo` | string (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `createdByUserName` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No | จำนวนเงินที่โอนแล้ว |
| `countItem` | integer (int32) | No | จำนวนรายการ |
| `additionalTransferLimit` | number (double) | No | จำนวนเงินโอนได้สูงสุด |
| `caseDetails` | array of [`CaseDetailDto`](#schema-casedetaildto) | No |  |
| `account` | [`accountDetail`](#schema-accountdetail) | No |  |

### SaveRefundDetailsResponseDtoServiceResponse {#schema-saverefunddetailsresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`SaveRefundDetailsResponseDto`](#schema-saverefunddetailsresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### SearchClaimOrCaseResponseDto {#schema-searchclaimorcaseresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseId` | string (uuid) | No |  |
| `claimId` | string (uuid) (nullable) | No |  |
| `claimCase` | string (nullable) | No |  |
| `createdClaimDate` | string (date-time) (nullable) | No | วันที่สร้างเคลม |
| `customerName` | string (nullable) | No |  |
| `coverageType` | string (nullable) | No | ประเภทความคุ้มครอง |
| `caseAmount` | number (double) (nullable) | No | จำนวนเงิน (case) |
| `isClaimNo` | boolean | No | รายการที่ค้นหาเป็นเลขที่ ClaimNo หรือไม |

### SearchClaimOrCaseResponseDtoListServiceResponse {#schema-searchclaimorcaseresponsedtolistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`SearchClaimOrCaseResponseDto`](#schema-searchclaimorcaseresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### StandardMedicalExpenseCategoryDtoResponse {#schema-standardmedicalexpensecategorydtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `inputToStandardCategoryId` | integer (int32) | No |  |
| `inputToStandardCategoryName` | string (nullable) | No |  |
| `inputToStandardSubCategoryList` | array of [`InputToStandardSubCategoryList`](#schema-inputtostandardsubcategorylist) | No |  |

### StandardMedicalExpenseCategoryDtoResponseListServiceResponse {#schema-standardmedicalexpensecategorydtoresponselistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`StandardMedicalExpenseCategoryDtoResponse`](#schema-standardmedicalexpensecategorydtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### SubmitHospitalBillingDto {#schema-submithospitalbillingdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `requestId` | string (uuid) | No |  |
| `expectedVersion` | integer (int32) | No |  |
| `expectedCaseVersion` | integer (int32) | No | Deprecated compatibility field; ระบบรับค่าไว้แต่ไม่ใช้ตรวจ Submit conflict. |
| `expectedClaimVersion` | integer (int32) | No | Deprecated compatibility field; ระบบรับค่าไว้แต่ไม่ใช้ตรวจ Submit conflict. |
| `rowVersion` | string (byte) | Yes |  |
| `caseRowVersion` | string (byte) | Yes | Deprecated compatibility field; ระบบรับค่าไว้แต่ไม่ใช้ตรวจ Submit conflict. |
| `claimRowVersion` | string (byte) | Yes | Deprecated compatibility field; ระบบรับค่าไว้แต่ไม่ใช้ตรวจ Submit conflict. |
| `reviewStatusId` | integer (int32) | No |  |
| `rejectReasonId` | integer (int32) (nullable) | No |  |
| `decisionReasonId` | integer (int32) (nullable) | No |  |
| `decisionId` | integer (int32) (nullable) | No |  |
| `reviewRemark` | string (nullable) | No |  |
| `data` | [`BillingReviewDataDto`](#schema-billingreviewdatadto) | Yes |  |

### TimeSpan {#schema-timespan}
| Field | Type | Required | Description |
|---|---|---|---|
| `ticks` | integer (int64) | No |  |
| `days` | integer (int32) | No |  |
| `hours` | integer (int32) | No |  |
| `milliseconds` | integer (int32) | No |  |
| `minutes` | integer (int32) | No |  |
| `seconds` | integer (int32) | No |  |
| `totalDays` | number (double) | No |  |
| `totalHours` | number (double) | No |  |
| `totalMilliseconds` | number (double) | No |  |
| `totalMinutes` | number (double) | No |  |
| `totalSeconds` | number (double) | No |  |

### TransferTransactionDetailResponseDto {#schema-transfertransactiondetailresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `paymentCode` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `paymentTypeName` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No |  |
| `toBankName` | string (nullable) | No |  |
| `toBankAccountNo` | string (nullable) | No |  |
| `toBankAccountName` | string (nullable) | No |  |

### TransferTransactionResponseDto {#schema-transfertransactionresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `payTransferDetails` | array of [`TransferTransactionDetailResponseDto`](#schema-transfertransactiondetailresponsedto) | No |  |

### TransferTransactionResponseDtoServiceResponse {#schema-transfertransactionresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`TransferTransactionResponseDto`](#schema-transfertransactionresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### UpdateAdditionalTransferRequestDto {#schema-updateadditionaltransferrequestdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `bankAccountRelationTypeId` | integer (int32) | Yes |  |
| `toBankId` | integer (int32) | Yes |  |
| `toBankAccountNo` | string | Yes |  |
| `toBankAccountName` | string | Yes |  |
| `totalNetPaidAmount` | number (double) (nullable) | No |  |
| `phoneNumber` | string | Yes |  |
| `paymentId` | string (uuid) | No |  |
| `caseId` | string (uuid) | No |  |
| `casePayableId` | string (uuid) | No |  |
| `claimId` | string (uuid) | No |  |
| `toBankName` | string (nullable) | No |  |

### UpdateAdditionalTransferResponseDto {#schema-updateadditionaltransferresponsedto}
| Field | Type | Required | Description |
|---|---|---|---|
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |

### UpdateAdditionalTransferResponseDtoServiceResponse {#schema-updateadditionaltransferresponsedtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`UpdateAdditionalTransferResponseDto`](#schema-updateadditionaltransferresponsedto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### UpdatePayTransferSettingRequestDto {#schema-updatepaytransfersettingrequestdto}
| Field | Type | Required | Description |
|---|---|---|---|
| `paytransferSettingId` | string (uuid) | Yes |  |

### UpdatePayTransferSettingRequestDtoServiceResponse {#schema-updatepaytransfersettingrequestdtoserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`UpdatePayTransferSettingRequestDto`](#schema-updatepaytransfersettingrequestdto) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### UpsertClaimDecisionBeneficiaryRequest {#schema-upsertclaimdecisionbeneficiaryrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `beneficiaryId` | string (uuid) | No |  |
| `policyBeneficiaryId` | integer (int32) | No |  |
| `titleId` | string (nullable) | No |  |
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `idCard` | string (nullable) | No |  |
| `phoneNo` | string (nullable) | No |  |
| `relationId` | integer (int32) (nullable) | No |  |
| `bankAccountRelationTypeId` | integer (int32) (nullable) | No |  |
| `bankId` | integer (int32) | No |  |
| `bankAccountNo` | string (nullable) | No |  |
| `bankAccountName` | string (nullable) | No |  |
| `payoutAmount` | number (double) | No |  |

### UpsertClaimDecisionCaseAdjudicationRequest {#schema-upsertclaimdecisioncaseadjudicationrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `decisionId` | integer (int32) | No |  |
| `decisionDate` | string (date-time) | No |  |
| `approvedAdmissionDate` | string (date-time) (nullable) | No |  |
| `approvedAdmissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `approvedDischargeDate` | string (date-time) (nullable) | No |  |
| `approvedDischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `coveredAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `compensateAmount` | number (double) | No |  |
| `approvedMedicalAmount` | number (double) | No |  |
| `approvedCompensateAmount` | number (double) | No |  |
| `patientPayAmount` | number (double) | No |  |
| `isExgratia` | boolean | No |  |
| `exgratiaAmount` | number (double) | No |  |
| `deductibleAmount` | number (double) | No |  |
| `coPayAmount` | number (double) | No |  |
| `coInsuranceAmount` | number (double) | No |  |
| `rejectReasonId` | integer (int32) (nullable) | No |  |
| `rejectDate` | string (date-time) (nullable) | No |  |
| `isLatest` | boolean | No |  |
| `approvedIPDDayCount` | integer (int32) | No |  |
| `approvedICUDayCount` | integer (int32) | No |  |
| `decisionReasonId` | integer (int32) (nullable) | No |  |
| `decisionRemark` | string (nullable) | No |  |
| `caseItemAdjudications` | array of [`UpsertClaimDecisionCaseItemAdjudicationRequest`](#schema-upsertclaimdecisioncaseitemadjudicationrequest) | No |  |

### UpsertClaimDecisionCaseAssessmentRequest {#schema-upsertclaimdecisioncaseassessmentrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `isDocumentComplete` | boolean | No |  |
| `documentReceivedDate` | string (date-time) | No |  |
| `documentCompleteDate` | string (date-time) | No |  |
| `isFraudSuspect` | boolean | No |  |
| `documentReceivedByUserId` | integer (int32) (nullable) | No |  |
| `documentReceivedByUserCode` | string (nullable) | No |  |
| `documentReceivedByUserName` | string (nullable) | No |  |

### UpsertClaimDecisionCaseDeathRequest {#schema-upsertclaimdecisioncasedeathrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDeathId` | string (uuid) | No |  |
| `causeOfIncidentId` | integer (int32) (nullable) | No |  |
| `deathDate` | string (date-time) | No |  |
| `deathTime` | [`TimeSpan`](#schema-timespan) | No |  |

### UpsertClaimDecisionCaseDisabilityRequest {#schema-upsertclaimdecisioncasedisabilityrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDisabilityId` | string (uuid) | No |  |
| `bodyPartId` | integer (int32) (nullable) | No |  |
| `disabilityTypeId` | integer (int32) (nullable) | No |  |
| `disabilityLevel` | integer (int32) (nullable) | No |  |
| `disabilityPercent` | integer (int32) (nullable) | No |  |

### UpsertClaimDecisionCaseDocumentDetailRequest {#schema-upsertclaimdecisioncasedocumentdetailrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `firstName` | string (nullable) | No |  |
| `lastName` | string (nullable) | No |  |
| `fullName` | string (nullable) | No |  |
| `hospitalName` | string (nullable) | No |  |
| `receiptAdmissionDate` | string (date-time) (nullable) | No |  |
| `receiptNumber` | string (nullable) | No |  |
| `receiptAmount` | number (double) (nullable) | No |  |
| `ocrDocumentTypeId` | integer (int32) (nullable) | No |  |
| `ocrResult` | string (nullable) | No |  |

### UpsertClaimDecisionCaseDocumentRequest {#schema-upsertclaimdecisioncasedocumentrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseDocumentId` | string (uuid) | No |  |
| `documentId` | string (uuid) | No |  |
| `documentNo` | string (nullable) | No |  |
| `documentSubTypeId` | integer (int32) | No |  |
| `documentReviewStatusId` | integer (int32) (nullable) | No |  |
| `documentReviewRemark` | string (nullable) | No |  |
| `caseDocumentDetail` | array of [`UpsertClaimDecisionCaseDocumentDetailRequest`](#schema-upsertclaimdecisioncasedocumentdetailrequest) | No |  |

### UpsertClaimDecisionCaseItemAdjudicationRequest {#schema-upsertclaimdecisioncaseitemadjudicationrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `standardMedicalExpenseId` | integer (int32) (nullable) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `eligibleAmount` | number (double) | No |  |
| `approvedAmount` | number (double) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `excessAmount` | number (double) | No |  |

### UpsertClaimDecisionCaseItemRequest {#schema-upsertclaimdecisioncaseitemrequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `inputToStandardMappingId` | integer (int32) | No |  |
| `standardMedicalExpenseId` | integer (int32) | No |  |
| `quantity` | integer (int32) | No |  |
| `perUnit` | integer (int32) | No |  |
| `originalAmount` | number (double) | No |  |
| `discountAmount` | number (double) | No |  |
| `netCaseAmount` | number (double) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nonCoveredAmount` | number (double) | No |  |
| `nonCoveredReasonId` | integer (int32) | No |  |

### UpsertClaimDecisionCaseRequest {#schema-upsertclaimdecisioncaserequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `coverageTypeId` | integer (int32) (nullable) | No |  |
| `occurrenceDate` | string (date-time) (nullable) | No |  |
| `occurrenceTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `admissionDate` | string (date-time) (nullable) | No |  |
| `admissionTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `dischargeDate` | string (date-time) (nullable) | No |  |
| `dischargeTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `caseAmount` | number (double) | No |  |
| `latestApprovedAmount` | number (double) | No |  |
| `latestNonCoveredAmount` | number (double) | No |  |
| `latestPatientPayAmount` | number (double) | No |  |
| `cancelReasonId` | integer (int32) (nullable) | No |  |
| `cancelDate` | string (date-time) (nullable) | No |  |
| `isCaseDisability` | boolean | No |  |
| `hospitalId` | integer (int32) (nullable) | No |  |
| `hn` | string (nullable) | No |  |
| `an` | string (nullable) | No |  |
| `vn` | string (nullable) | No |  |
| `chiefComplaintId` | integer (int32) (nullable) | No |  |
| `chiefComplaintCustom` | string (nullable) | No |  |
| `productId` | integer (int32) (nullable) | No |  |
| `icD10_1stId` | integer (int32) (nullable) | No |  |
| `icD10_2ndId` | integer (int32) (nullable) | No |  |
| `icD10_3rdId` | integer (int32) (nullable) | No |  |
| `icD10_4thId` | integer (int32) (nullable) | No |  |
| `icD10_5thId` | integer (int32) (nullable) | No |  |
| `icD10_6thId` | integer (int32) (nullable) | No |  |
| `medicalTypeId` | integer (int32) (nullable) | No |  |
| `nplAmount` | number (double) (nullable) | No |  |
| `insuranceDiscountAmount` | number (double) (nullable) | No |  |
| `customerDiscountAmount` | number (double) (nullable) | No |  |
| `caseItem` | array of [`UpsertClaimDecisionCaseItemRequest`](#schema-upsertclaimdecisioncaseitemrequest) | No |  |
| `caseAssessment` | [`UpsertClaimDecisionCaseAssessmentRequest`](#schema-upsertclaimdecisioncaseassessmentrequest) | No |  |
| `caseAdjudication` | [`UpsertClaimDecisionCaseAdjudicationRequest`](#schema-upsertclaimdecisioncaseadjudicationrequest) | No |  |
| `caseDeath` | array of [`UpsertClaimDecisionCaseDeathRequest`](#schema-upsertclaimdecisioncasedeathrequest) | No |  |
| `caseDisability` | array of [`UpsertClaimDecisionCaseDisabilityRequest`](#schema-upsertclaimdecisioncasedisabilityrequest) | No |  |
| `beneficiary` | array of [`UpsertClaimDecisionBeneficiaryRequest`](#schema-upsertclaimdecisionbeneficiaryrequest) | No |  |
| `caseDocument` | array of [`UpsertClaimDecisionCaseDocumentRequest`](#schema-upsertclaimdecisioncasedocumentrequest) | No |  |

### UpsertClaimDecisionDtoRequest {#schema-upsertclaimdecisiondtorequest}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimId` | string (uuid) | No |  |
| `caseId` | string (uuid) | No |  |
| `incidentTypeId` | integer (int32) | No |  |
| `incidentDate` | string (date-time) | No |  |
| `incidentTime` | [`TimeSpan`](#schema-timespan) | No |  |
| `accidentPlace` | string (nullable) | No |  |
| `accidentDescription` | string (nullable) | No |  |
| `case` | [`UpsertClaimDecisionCaseRequest`](#schema-upsertclaimdecisioncaserequest) | No |  |

### UpsertClaimDecisionDtoResponse {#schema-upsertclaimdecisiondtoresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `isResult` | boolean | No |  |
| `result` | string (nullable) | No |  |
| `msg` | string (nullable) | No |  |
| `claimId` | string (uuid) (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `caseId` | string (uuid) (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `casePayableId` | string (uuid) (nullable) | No |  |

### UpsertClaimDecisionDtoResponseServiceResponse {#schema-upsertclaimdecisiondtoresponseserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | [`UpsertClaimDecisionDtoResponse`](#schema-upsertclaimdecisiondtoresponse) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### usp_AdditionalTransferMonitor_SelectResult {#schema-usp_additionaltransfermonitor_selectresult}
| Field | Type | Required | Description |
|---|---|---|---|
| `caseId` | string (uuid) | No |  |
| `claimId` | string (uuid) | No |  |
| `claimNo` | string (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `customerName` | string (nullable) | No |  |
| `branchName` | integer (int32) (nullable) | No |  |
| `paymentStatusNameTH` | string (nullable) | No |  |
| `remark` | integer (int32) (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No |  |
| `addPayAmount` | number (double) (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |
| `paymentStatusId` | integer (int32) (nullable) | No |  |
| `casePayableId` | string (uuid) | No |  |
| `paymentId` | string (uuid) | No |  |

### usp_AdditionalTransferMonitor_SelectResultListServiceResponse {#schema-usp_additionaltransfermonitor_selectresultlistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`usp_AdditionalTransferMonitor_SelectResult`](#schema-usp_additionaltransfermonitor_selectresult) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### usp_FailedPayTransferTransaction_SelectResult {#schema-usp_failedpaytransfertransaction_selectresult}
| Field | Type | Required | Description |
|---|---|---|---|
| `claimNo` | string (nullable) | No |  |
| `caseNo` | string (nullable) | No |  |
| `claimCreated` | string (date-time) (nullable) | No |  |
| `toAccountNo` | string (nullable) | No |  |
| `toBank` | string (nullable) | No |  |
| `toAccountName` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No |  |
| `paymentStatusId` | integer (int32) (nullable) | No |  |
| `payTransferTransactionId` | string (uuid) (nullable) | No |  |
| `payListHeaderId` | string (uuid) (nullable) | No |  |
| `paymentCode` | string (nullable) | No |  |
| `paymentId` | string (uuid) | No |  |
| `paymentStatusNameTH` | string (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### usp_FailedPayTransferTransaction_SelectResultListServiceResponse {#schema-usp_failedpaytransfertransaction_selectresultlistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`usp_FailedPayTransferTransaction_SelectResult`](#schema-usp_failedpaytransfertransaction_selectresult) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |

### usp_InquiryMonitor_SelectResult {#schema-usp_inquirymonitor_selectresult}
| Field | Type | Required | Description |
|---|---|---|---|
| `paymentId` | string (uuid) | No |  |
| `toAccountNo` | string (nullable) | No |  |
| `toAccountName` | string (nullable) | No |  |
| `toBank` | string (nullable) | No |  |
| `totalNetPaidAmount` | number (double) (nullable) | No |  |
| `paymentCode` | string (nullable) | No |  |
| `createdDate` | string (date-time) (nullable) | No |  |
| `claimNo` | string (nullable) | No |  |
| `payTransferTransactionId` | string (uuid) (nullable) | No |  |
| `transferStatusId` | integer (int32) (nullable) | No |  |
| `transferStatusName` | string (nullable) | No |  |
| `payListHeaderId` | string (uuid) (nullable) | No |  |
| `totalCount` | integer (int32) (nullable) | No |  |

### usp_InquiryMonitor_SelectResultListServiceResponse {#schema-usp_inquirymonitor_selectresultlistserviceresponse}
| Field | Type | Required | Description |
|---|---|---|---|
| `data` | array of [`usp_InquiryMonitor_SelectResult`](#schema-usp_inquirymonitor_selectresult) | No |  |
| `isSuccess` | boolean | No |  |
| `message` | string (nullable) | No |  |
| `code` | integer (int32) (nullable) | No |  |
| `exceptionMessage` | object | No |  |
| `serverDateTime` | string (date-time) | No |  |
| `totalAmountRecords` | number (double) (nullable) | No |  |
| `totalAmountPages` | number (double) (nullable) | No |  |
| `currentPage` | number (double) (nullable) | No |  |
| `recordsPerPage` | number (double) (nullable) | No |  |
| `pageIndex` | integer (int32) (nullable) | No |  |
