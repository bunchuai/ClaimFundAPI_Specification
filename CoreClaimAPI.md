# CoreClaim API Specification

- **Title:** CoreClaim_api
- **Version:** v1
- **OpenAPI:** 3.0.1
- **Source:** `https://78nq1mrd-5001.asse.devtunnels.ms/swagger/v1/swagger.json`

> **หมายเหตุ:** เอกสารนี้สร้างจากข้อมูล `paths` ที่ดึงได้จาก swagger.json ไฟล์ต้นฉบับมีขนาดใหญ่มาก ส่วน `components/schemas` (โครงสร้างฟิลด์แบบละเอียดของแต่ละ DTO) ยังไม่ได้ถูกดึงมาครบ เอกสารนี้จึงแสดงชื่อ Schema อ้างอิง (`$ref`) ของแต่ละ endpoint แทนรายละเอียดฟิลด์ทั้งหมด หากต้องการรายละเอียด field-level แนะนำให้อัปโหลดไฟล์ swagger.json ฉบับเต็มเพื่อให้ผมเจาะรายละเอียด schema เพิ่มเติมได้

---

## สารบัญ

1. [ภาพรวม](#ภาพรวม)
2. [Authentication](#authentication)
3. [กลุ่ม ClaimFund API](#กลุ่ม-claimfund-api)
   - 3.1 [Masters](#31-masters)
   - 3.2 [Setting](#32-setting)
   - 3.3 [IncreaseTransfer](#33-increasetransfer)
   - 3.4 [AdditionalTransfer](#34-additionaltransfer)
   - 3.5 [Inquiry](#35-inquiry)
   - 3.6 [FailedPayTransfer](#36-failedpaytransfer)
   - 3.7 [Refund](#37-refund)
4. [กลุ่ม CoreClaim API](#กลุ่ม-coreclaim-api)
   - 4.1 [Customer](#41-customer)
   - 4.2 [Employee Payment Limit](#42-employee-payment-limit)
   - 4.3 [Calculate](#43-calculate)
   - 4.4 [Create](#44-create)
   - 4.5 [Document](#45-document)
   - 4.6 [Claim](#46-claim)
   - 4.7 [Dashboard](#47-dashboard)
   - 4.8 [Policy](#48-policy)
   - 4.9 [DCR](#49-dcr)
   - 4.10 [Standard Medical Expense](#410-standard-medical-expense)
5. [หมายเหตุ / ข้อจำกัดของเอกสารนี้](#หมายเหตุ--ข้อจำกัดของเอกสารนี้)

---

## ภาพรวม

API นี้แบ่งเป็น 2 กลุ่มหลักตาม tag:

| กลุ่ม (Tag) | คำอธิบาย |
|---|---|
| `ClaimFund` | จัดการการโอนเงิน/คืนเงินค่าสินไหมทดแทน (การตั้งค่า, โอนเพิ่ม, สอบถามธนาคาร, การคืนเงิน) |
| `CoreClaim` | จัดการข้อมูลลูกค้า เคลม เอกสาร การคำนวณสินไหม และ dashboard ต่าง ๆ |

## Authentication

Endpoint ส่วนใหญ่ในกลุ่ม `ClaimFund` ระบุ security scheme เป็น **OAuth2** (ดูจาก `"security": [{ "OAuth2": [] }]` ในแต่ละ endpoint) และคืนค่า response `401 Unauthorized` / `403 Forbidden` เป็นมาตรฐาน ส่วน endpoint ในกลุ่ม `CoreClaim` ที่ดึงมาได้ยังไม่ระบุ security scheme ชัดเจนในข้อมูลที่มี

---

## กลุ่ม ClaimFund API

### 3.1 Masters

| Method | Path | Summary | Parameters | Response Schema |
|---|---|---|---|---|
| GET | `/api/ClaimFund/Masters/GetAdjustmentReasons` | ข้อมูลสาเหตุการโอนเพิ่ม | `AdjustmentTypeId` (query, int32) | `AdjustmentReasonResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/Masters/GetAdjustmentReasonById` | ข้อมูลสาเหตุการโอนเพิ่ม By Id | `AdjustmentReasonId` (query, int32) | `AdjustmentReasonResponseDtoServiceResponse` |
| GET | `/api/ClaimFund/Masters/GetPaymentStatuses` | ข้อมูลสถานะการจ่ายเงิน (Payment Status) | - | `PaymentStatusResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/Masters/GetBankAccountRelationTypes` | ข้อมูลประเภทความสัมพันธ์ของบัญชีกับผู้รับสินไหม | - | `BankAccountRelationTypeResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/Masters/GetBankAccountRelationTypeById` | ข้อมูลประเภทความสัมพันธ์ของบัญชีกับผู้รับสินไหม By Id | `bankAccountRelationTypeId` (query, int32) | `BankAccountRelationTypeResponseDtoServiceResponse` |
| GET | `/api/ClaimFund/Masters/GetRefundReasons` | ข้อมูลสาเหตุการโอนคืน | - | `RefundReasonResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/Masters/GetRefundReasonById` | ข้อมูลสาเหตุการโอนคืน By Id | `refundReasonId` (query, int32) | `RefundReasonResponseDtoServiceResponse` |
| GET | `/api/ClaimFund/Masters/GetRefundStatus` | สถานะการคืนเงิน | - | `RefundStatusResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/Masters/GetCaseRefundRejectReasons` | สาเหตุที่ปฏิเสธ | - | `CaseRefundRejectReasonResponseDtoListServiceResponse` |

### 3.2 Setting

| Method | Path | Summary | Parameters / Request Body | Response Schema |
|---|---|---|---|---|
| GET | `/api/ClaimFund/Setting/GetCurrentSetting` | ตั้งค่าการโอนเงิน | - | `PayTransferSettingResponseDtoServiceResponse` |
| POST | `/api/ClaimFund/Setting/UpdateSettingAutoTransfer` | บันทึกการตั้งค่าการโอนเงิน | Body: `UpdatePayTransferSettingRequestDto` | `UpdatePayTransferSettingRequestDtoServiceResponse` |
| GET | `/api/ClaimFund/Setting/SearchClaimOrCase` | ค้นหารายการจากเลขที่ ClaimNo / CaseNo | `searchDetail` (query, string, **required**) | `SearchClaimOrCaseResponseDtoListServiceResponse` |

### 3.3 IncreaseTransfer

| Method | Path | Summary | Parameters | Response Schema |
|---|---|---|---|---|
| GET | `/api/ClaimFund/IncreaseTransfer/IncreaseTransferLimitMonitors` | Monitor ขยายวงเงิน | `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `IncreaseTransferLimitMonitorResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/IncreaseTransfer/GetIncreaseTransferLimitDetail` | รายละเอียดขยายวงเงิน | `caseTransferApprovalId` (query, uuid) | `GetIncreaseTransferLimitDetailResponseDtoServiceResponse` |

### 3.4 AdditionalTransfer

| Method | Path | Summary | Parameters / Request Body | Response Schema |
|---|---|---|---|---|
| GET | `/api/ClaimFund/AdditionalTransfer/GetAdditionalTransferAccountDetail` | รายละเอียดบัญชี (แก้ไขการโอนเงิน) | `paymentId` (query, uuid) | `AdditionalTransferAccountDetailsResponseDtoListServiceResponse` |
| POST | `/api/ClaimFund/AdditionalTransfer/AdditionalTransferMonitor` | ตรวจสอบรายการโอนเงินเพิ่ม (Monitor) | Query: `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage`; Body: `AdditionalTransferMonitorRequestDto` | `usp_AdditionalTransferMonitor_SelectResultListServiceResponse` |
| GET | `/api/ClaimFund/AdditionalTransfer/GetClaimTransaction` | ดึงประวัติการทำรายการ (Transaction) ของเคลม | `caseId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `AdditionalTransferTransactionResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/AdditionalTransfer/AdditionalTransferDetails` | โอนเพิ่ม รายละเอียด step(1) | `caseId` (query, uuid) | `AdditionalTransferDetailsResponseDtoServiceResponse` |
| GET | `/api/ClaimFund/AdditionalTransfer/GetClaimTransactions` | ประวัติการทำรายการ | `caseId`, `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `AdditionalTransferTransactionResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/AdditionalTransfer/TransferHistory` | ประวัติการโอนเงิน | `caseId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `TransferTransactionResponseDtoServiceResponse` |
| GET | `/api/ClaimFund/AdditionalTransfer/GetDecreaseTransaction` | ประวัติการลดยอด (โอนเพิ่ม) | `caseId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetDecreaseTransactionResponseDtoListServiceResponse` |
| POST | `/api/ClaimFund/AdditionalTransfer/UpdateAdditionalTransfer` | แก้ไขการโอนเงินเพิ่ม | Body: `UpdateAdditionalTransferRequestDto` | `UpdateAdditionalTransferResponseDtoServiceResponse` |
| POST | `/api/ClaimFund/AdditionalTransfer/SaveAdditionalTransfer` | บันทึกการโอนเงินเพิ่ม | Body: `SaveAdditionalTransferRequest` | `SaveAdditionalTransferResponseDtoServiceResponse` |

### 3.5 Inquiry

| Method | Path | Summary | Parameters | Response Schema |
|---|---|---|---|---|
| GET | `/api/ClaimFund/Inquiry/InquiryMonitors` | สอบถามธนาคาร Inquiry Monitor | `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `usp_InquiryMonitor_SelectResultListServiceResponse` |
| GET | `/api/ClaimFund/Inquiry/InquiryDetail` | สอบถามธนาคาร Inquiry Monitor รายละเอียด | `PayTransferTransactionId` (query, uuid, **required**) | `BankInquiryDetailResponseDtoServiceResponse` |

### 3.6 FailedPayTransfer

| Method | Path | Summary | Parameters | Response Schema |
|---|---|---|---|---|
| GET | `/api/ClaimFund/FailedPayTransfer/FailedPayTransferTransactionMonitor` | Monitor - แก้ไขการโอนเงิน | `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `usp_FailedPayTransferTransaction_SelectResultListServiceResponse` |
| GET | `/api/ClaimFund/FailedPayTransfer/FailedPayTransferTransactionDetail` | รายละเอียด Monitor - แก้ไขการโอนเงิน | `PayTransferTransactionId` (query, uuid, **required**) | `FailedPayTransferTransactionResponseDtoServiceResponse` |

### 3.7 Refund

| Method | Path | Summary | Parameters / Request Body | Response Schema |
|---|---|---|---|---|
| GET | `/api/ClaimFund/Refund/SaveRefundDetails` | รายละเอียดการคืนเงิน (Refund) | `caseId` (query, uuid, **required**) | `SaveRefundDetailsResponseDtoServiceResponse` |
| GET | `/api/ClaimFund/Refund/SaveRefundAccountDetail` | รายละเอียดบัญชีสำหรับการคืนเงิน (Refund) | `paymentId` (query, uuid, **required**) | `RefundDetailsAccountDetailsResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/Refund/GetClaimTransaction` | ดึงประวัติการทำรายการของเคลม สำหรับ Refund | `caseId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `RefundTransactionResponseDtoListServiceResponse` |
| GET | `/api/ClaimFund/Refund/TransferHistory` | ประวัติการโอนเงิน สำหรับ Refund | `caseId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `RefundTransferTransactionResponseDtoServiceResponse` |
| GET | `/api/ClaimFund/Refund/GetDecreaseTransaction` | ประวัติการลดยอด สำหรับ Refund | `caseId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetDecreaseTransactionRefundResponseDtoListServiceResponse` |
| POST | `/api/ClaimFund/Refund/RefundMonitor` | Monitor การคืนเงิน (Refund) | Query: paging/search params; Body: `RefundMonitorRequestDto` | `RefundMonitorResponseListServiceResponse` |
| POST | `/api/ClaimFund/Refund/RefundApproveMonitor` | Monitor approve | Query: paging/search params; Body: `RefundApproveMonitorRequestDto` | `RefundApproveMonitorResponseListServiceResponse` |
| POST | `/api/ClaimFund/Refund/CreateCaseRefund` | สร้างรายการเงินคืนเงิน | Body: `CreateRefundRequestDto` | `CreateRefundResponsetDtoServiceResponse` |
| GET | `/api/ClaimFund/Refund/CaseRefundApproveDetail` | รายละเอียดการอนุมัติคืนเงิน | `caseRefundId` (query, uuid) | `CaseRefundApproveDetailResponseDtoServiceResponse` |
| POST | `/api/ClaimFund/Refund/CaseRefundApproveUpdateStatus` | อนุมัติคืนเงิน / ปฏิเสธ | Body: `CaseRefundApproveUpdateStatusRequestDto` | `CaseRefundApproveUpdateStatusResponseDtoServiceResponse` |

---

## กลุ่ม CoreClaim API

### 4.1 Customer

| Method | Path | Summary | operationId | Response Schema |
|---|---|---|---|---|
| GET | `/api/customer/search` | Search Customer | `GetCustomerSearch` | `GetCustomerSearchDtoResponseListServiceResponse` |
| GET | `/api/customer/search-by-policy-code` | Search Customer By PolicyCode | `GetCustomerSearchByPolicyCode` | `GetCustomerSearchByPolicyCodeDtoResponseListServiceResponse` |
| GET | `/api/customer/{customerDetailId}/detail` | Get ข้อมูล Customer Detail By Id | `GetCustomerDetailById` | `GetCustomerDetailByIdDtoResponseServiceResponse` |
| GET | `/api/customer/benefit-detail/search` | Search Customer Benefit Detail | `GetCustomerBenefitDetailSearch` | `GetCustomerBenefitDetailSearchDtoResponseListServiceResponse` |
| GET | `/api/customer/benefit-detail/half` | Get Customer Benefit Detail Half | `GetCustomerBenefitDetailHalf` | `GetCustomerBenefitDetailHalfDtoResponseListServiceResponse` |
| GET | `/api/customer/{policyCode}/bank-account` | Get ข้อมูล Customer BankAccount | `GetCustomerBankAccount` | `GetCustomerBankAccountDtoResponseListServiceResponse` |
| GET | `/api/customer/contact-person` | Get ข้อมูล Contact Person | `GetContactPerson` | `GetContactPersonDtoResponseListServiceResponse` |
| GET | `/api/customer/policybenefit-shered` | Get ข้อมูล Policy Benefit Shered (สิทธิประโยชน์ร่วม) | `GetPolicyBenefitShered` | `GetPolicyBenefitSheredDtoResponseListServiceResponse` |

**พารามิเตอร์หลักที่ใช้บ่อยในกลุ่มนี้:** `policyCode`, `customerDetailId` (uuid, path), `incidentDate` (date-time), `incidentTypeId`, `coverageTypeId`, `medicalTypeId`, `claimNo`, `customerTypeCode`, รวมถึงชุด paging มาตรฐาน `searchDetail` / `orderingField` / `ascendingOrder` / `Page` / `recordsPerPage`

### 4.2 Employee Payment Limit

| Method | Path | Summary | operationId | Parameters | Response Schema |
|---|---|---|---|---|---|
| GET | `/api/employee-payment-limit/{userId}` | ตรวจวงเงินรายวันก่อนบันทึก Case โดยใช้วันที่จาก request หากระบุ มิฉะนั้นใช้วันที่ปัจจุบันของ Server และไม่จองวงเงิน | `GetEmployeeClaimPaymentLimit` | `userId` (path, int32, 1–2147483647, **required**), `RequestedTransferAmount` (query, double, **required**), `RequestedDate` (query, date-time) | `GetEmployeeClaimPaymentLimitResponseServiceResponse` |

### 4.3 Calculate

| Method | Path | Summary | operationId | Parameters / Body | Response Schema |
|---|---|---|---|---|---|
| POST | `/api/calculate/caseclaim` | Calculate ข้อมูล Case Claim | `CalculateCaseClaim` | Body: `CalculateCaseClaimDtoRequest` | `CalculateCaseClaimDtoResponseServiceResponse` |
| GET | `/api/calculate/disability` | Calculate ข้อมูล Case Disability (สูญเสียอวัยวะ) | `CalculateCaseDisability` | `customerDetailId` (uuid), `bodyPartId` (int32), `standardMedicalExpenseId` (int32) | `CalculateCaseDisabilityDtoResponseServiceResponse` |

### 4.4 Create

| Method | Path | Summary | operationId | Request Body | Response Schema |
|---|---|---|---|---|---|
| POST | `/api/create/coreclaim` | Create ข้อมูล CoreClaim | `CreateCoreClaim` | `CreateCoreClaimV2DtoRequest` | `CreateCoreClaimDtoResponseServiceResponse` |
| POST | `/api/create/continued-claim` | เพิ่ม Case ใหม่ภายใต้ Claim เดิม โดยอ้างอิง ClaimId | `CreateContinuedClaim` | `CreateContinuedClaimDtoRequest` | `CreateCoreClaimDtoResponseServiceResponse` |
| POST | `/api/create/case-adjudication` | สร้างผลการพิจารณาเริ่มต้นของ Case โดยอ้างอิง CaseId | `CreateCaseAdjudication` | `CreateCaseAdjudicationDtoRequest` | `CreateCaseAdjudicationDtoResponseServiceResponse` |

### 4.5 Document

| Method | Path | Summary | operationId | Parameters / Body | Response Schema |
|---|---|---|---|---|---|
| POST | `/api/document-subtype` | Get ข้อมูล DocumentSubType (`documentTypeId`: เอกสารของโปรเจค, `documentPrefix`: คำนำหน้ารหัสเอกสาร, `documentSubTypeIdList`: รายการรหัสเอกสารย่อยที่ต้องการแสดง) | `GetDocumentSubType` | Body: `GetDocumentSubTypeDtoRequest` | `GetDocumentSubTypeDtoResponseListServiceResponse` |
| GET | `/api/document/case/filter` | Get ข้อมูล Document By CaseId | `GetDocumentByCaseId` | `caseId` (uuid), `productTypeId`, `claimSourceId`, `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetDocumentByCaseIdDtoResponseListServiceResponse` |
| GET | `/api/document/case/{caseId}/overview` | แสดงข้อมูลภาพรวมการตรวจสอบเอกสาร ผลการพิจารณา และรายการค่าใช้จ่ายของ Case | `GetCaseReviewOverview` | `caseId` (path, uuid, **required**) | `GetCaseReviewOverviewDtoResponseServiceResponse` |

### 4.6 Claim

| Method | Path | Summary | operationId | Parameters | Response Schema |
|---|---|---|---|---|---|
| GET | `/api/claim/history/filter` | Get ข้อมูลประวัติการเคลม | `GetClaimHistory` | `applicationId`, `incidentTypeId`, `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetClaimHistoryDtoResponseListServiceResponse` |
| GET | `/api/claim/continue/filter` | Get ข้อมูลการเคลมต่อเนื่อง | `GetClaimContinue` | `applicationId`, `initialCaseId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetClaimContinueDtoResponseListServiceResponse` |
| GET | `/api/claim/case/filter` | Get ข้อมูล Case By ClaimId | `GetCaseByClaimId` | `claimId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetCaseByClaimIdDtoResponseListServiceResponse` |
| GET | `/api/claim/customer-monitor/filter` | Get ข้อมูล CustomerClaim Adjudication Monitor | `GetCustomerClaimAdjudicationMonitor` | `dateOption`, `dateFrom`, `dateTo`, `isProductTypeId_PH`, `isProductTypeId_PA`, `claimTransactionTypeId`, `searchOption`, `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetCustomerClaimAdjudicationMonitorDtoResponseListServiceResponse` |
| GET | `/api/claim/hospital-monitor/filter` | Get ข้อมูล HospitalClaim Adjudication Monitor | `GetHospitalClaimAdjudicationMonitor` | (เหมือน customer-monitor ด้านบน) | `GetHospitalClaimAdjudicationMonitorDtoResponseListServiceResponse` |
| GET | `/api/claim/detail/consider/{claimId}/{caseId}` | Get ข้อมูลรายละเอียด Claim พิจารณา | `GetClaimDetailConsider` | `claimId` (path, uuid, **required**), `caseId` (path, uuid, **required**) | `GetClaimDetailConsiderDtoResponseServiceResponse` |
| GET | `/api/claim/{claimId}/previous` | Get ข้อมูลเคลมก่อนหน้า | `GetPreviousClaim` | `claimId` (path, uuid, **required**) | `GetPreviousClaimDtoResponseServiceResponse` |
| GET | `/api/claim/transaction-log/filter` | Get ข้อมูล TransactionLog Claim (ประวัติการทำรายการ) | `GetClaimTransactionLog` | `claimId` (uuid), `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetClaimTransactionLogDtoResponseListServiceResponse` |

### 4.7 Dashboard

| Method | Path | Summary | operationId | Parameters | Response Schema |
|---|---|---|---|---|---|
| GET | `/api/dashboard/customer-consider/filter` | Get ข้อมูล Dashboard Customer Consider (พิจารณาเคลมลูกค้า) | `GetDashboardCustomerConsider` | `dateOption`, `dateFrom`, `dateTo` | `GetDashboardCustomerConsiderDtoResponseListServiceResponse` |

### 4.8 Policy

| Method | Path | Summary | operationId | Parameters | Response Schema |
|---|---|---|---|---|---|
| GET | `/api/policy/benefit` | Get ข้อมูล PolicyBenefit (ความคุ้มครอง) | `GetPolicyBenefit` | `productTypeId`, `applicationCode`, `productId`, `customerTypeCode` | `GetPolicyBenefitDtoResponseListServiceResponse` |

### 4.9 DCR

| Method | Path | Summary | operationId | Parameters | Response Schema |
|---|---|---|---|---|---|
| GET | `/api/dcr/filter` | Get ข้อมูล DCR (การชำระเงิน) | `GetDCR` | `applicationCode`, `searchDetail`, `orderingField`, `ascendingOrder`, `Page`, `recordsPerPage` | `GetDCRDtoResponseListServiceResponse` |

### 4.10 Standard Medical Expense

| Method | Path | Summary | operationId | Parameters | Response Schema |
|---|---|---|---|---|---|
| GET | `/api/standard-medical-expense/case` | Get ข้อมูลรายการค่ารักษา (เบื้องต้น) Default จาก CaseItem จากหน้าแจ้งเคลม | `GetStandardMedicalExpenseByCase` | `caseId` (query, uuid, **required**), `formatTypeId`, `coverageTypeId`, `medicalTypeId` (*และอาจมี parameter เพิ่มเติมที่ถูกตัดออกจากข้อมูลต้นฉบับ*) | *(ไม่ทราบ — ข้อมูลถูกตัดก่อนถึงส่วน responses)* |

---

## หมายเหตุ / ข้อจำกัดของเอกสารนี้

- เอกสารนี้สร้างจากการดึงข้อมูล swagger.json ผ่าน web fetch ซึ่งไฟล์มีขนาดใหญ่เกินกว่าจะดึงมาได้ครบในครั้งเดียว **ส่วนที่ขาดหายไป** ได้แก่:
  - Endpoints ที่อยู่หลังจาก `/api/standard-medical-expense/case` (ถ้ามี)
  - ส่วน `components/schemas` ทั้งหมด (โครงสร้างฟิลด์ของแต่ละ DTO เช่น `CreateCoreClaimV2DtoRequest`, `CalculateCaseClaimDtoRequest` ฯลฯ)
  - รายละเอียด security scheme configuration (`components/securitySchemes`)
- หากต้องการเอกสารฉบับสมบูรณ์ 100% (รวม field-level schema ของทุก DTO) แนะนำให้ดาวน์โหลดไฟล์ `swagger.json` แล้วอัปโหลดเข้ามาในแชทโดยตรง ผมจะประมวลผลไฟล์เต็มและเพิ่มรายละเอียด schema ให้ครบถ้วน
