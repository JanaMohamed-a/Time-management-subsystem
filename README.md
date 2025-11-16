# Time Management Subsystem

This repository implements the **Time Management** subsystem for the HR Management System project.  
It is responsible for managing shifts, schedules, attendance, overtime, lateness, and related approvals.

## Responsibilities

- Configure shift types (normal, split, overnight, rotational, mission) and custom scheduling rules.
- Assign shifts to employees, departments, or positions.
- Capture daily attendance (clock-in/out) from different sources (biometric, web, mobile, manual).
- Detect missed punches, lateness, overtime, short time, and absences.
- Support employee attendance correction requests and time exception requests (overtime, permissions).
- Integrate with:
  - **Employee Profile** (employeeId)
  - **Organizational Structure** (departmentId, positionId, managerId)
  - **Leaves** (leaveRequestId, holidays, rest days)
  - **Payroll** (payrollPeriodId, locking attendance for payroll)

## Tech Stack

- **Backend:** NestJS + TypeScript  
- **Database:** MongoDB (via Mongoose)  
- **Config:** `@nestjs/config` for `.env`  
- **This repo:** Time Management subsystem only. Later it will be merged into the main HR repo.

## MongoDB Schemas (Models)

All models are defined under:

`src/time-management/schemas/`

- `ShiftType` – defines all shift templates (name, category, core hours, grace period, punch mode).
- `SchedulingRule` – defines flexible / compressed / rotational schedules and weekly patterns.
- `ShiftAssignment` – assigns a shift (and optional scheduling rule) to an employee, department, or position.
- `OrganizationCalendarDay` – defines national/company holidays and blocked periods.
- `RestDayConfig` – defines weekly rest days at company/department/employee level.
- `AttendanceRecord` – stores daily attendance details (punches, lateness, overtime, leave linkage, payroll lock).
- `AttendanceCorrectionRequest` – employee/manager/HR workflow to adjust incorrect attendance.
- `OvertimeRule` – configuration for overtime multipliers and approval requirements.
- `LatenessRule` – configuration for grace periods, thresholds, and penalty formulas.
- `PermissionRule` – rules for early leave, late arrival, and other time permissions.
- `TimeExceptionRequest` – unified workflow for overtime and permission requests with approvals and escalation.
- `IntegrationSyncLog` – tracks synchronization of attendance/time data to **Payroll** and **Leaves** subsystems.

## External References (Integration)

The following fields reference collections defined in other subsystems (main HR repo):

- `employeeId` → `Employee` (Employee Profile subsystem)
- `departmentId` → `Department` (Organizational Structure subsystem)
- `positionId` → `Position` (Organizational Structure subsystem)
- `managerId` → `Employee` (manager role; from Org Structure)
- `hrReviewerId`, `createdById`, `lastUpdatedById` → `User` (Auth/User subsystem)
- `leaveRequestId` → `LeaveRequest` (Leaves subsystem)
- `payrollPeriodId` → `PayrollPeriod` (Payroll subsystem)

In this standalone repo these are stored as `ObjectId` with string `ref` values, so they will plug into the main HR repository later without schema changes.

## Running the Subsystem Locally

1. Install dependencies:

   ```bash
   npm install
