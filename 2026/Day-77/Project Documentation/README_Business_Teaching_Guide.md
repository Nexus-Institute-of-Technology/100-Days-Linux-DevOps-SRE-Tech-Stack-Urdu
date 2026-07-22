# NIT Bank Linux Process Operations Lab

## Business Project Document Pack

This pack turns the Linux Process Management technical lab into a small business project. Students do not only run commands; they learn how a company defines scope, plans resources, controls change, manages risk, validates results, communicates incidents and closes a project.

## Recommended Teaching Order

### 1. Business Need and Project Initiation

Start with the scenario:

> NIT Bank wants to train junior Linux operations engineers to support a report-generation service. A controlled classroom project will be used to teach process monitoring, job control, signals, CPU troubleshooting, priority, systemd and incident recovery.

Use these documents first:

1. **CTR** – defines what is being delivered, how long it will take, who is required and the estimated cost.
2. **PMP** – defines project governance, roles, communication, risk, quality and control methods.
3. **Master Schedule** – defines the work breakdown structure, dates, milestones and progress reporting.
4. **ITQ Register** – records missing business and technical information that must be supplied by an owner.

### 2. Design and Change Approval

Use:

1. **FDS** – explains how the requirements will be implemented and tested.
2. **NCD** – explains how the change will be executed, validated and rolled back.
3. **Master Document List** – controls file names, revisions, owners and document status.

Teach students that technical work should not begin until critical prerequisites are confirmed and the project receives a **Go** decision.

### 3. Technical Implementation

Students follow the approved NCD and technical lab guide to:

- Create the project scripts.
- Identify PID, PPID, process owner and state.
- Use foreground and background jobs.
- Test SIGSTOP, SIGCONT, SIGTERM and SIGKILL.
- Simulate and resolve high CPU usage.
- Use `nice` and `renice`.
- Test `nohup` across logout.
- Inspect `/proc`.
- Create parent-child and zombie processes.
- Create a least-privilege `systemd` service.

Every completed step should have evidence.

### 4. Incident Management

Use the bank scenario:

> The reporting server is slow. A process is consuming excessive CPU and the report service may be unavailable.

Students must explain the incident in business terms:

- What service was affected?
- What was the user or business impact?
- How was the fault identified?
- What was the root cause?
- What corrective action was taken?
- How was recovery validated?
- What preventive action is recommended?

### 5. Acceptance and Project Closure

A project is not complete because the commands worked. It is complete when:

- Mandatory validation tests pass.
- Evidence is complete and traceable.
- The incident report is submitted.
- Open ITQs and risks are closed or formally accepted.
- Final documents are issued with correct revisions.
- The instructor or business owner signs the acceptance record.
- Lessons learned are documented.

## Business Terms Students Should Learn

| Term | Meaning in This Project |
|---|---|
| Scope | The approved technical and business work included in the project. |
| Deliverable | A document, system component, test result or handover item produced by the project. |
| Assumption | Something believed to be true for planning but not yet fully confirmed. |
| Dependency | An input or condition required before another task can start or finish. |
| ITQ | A controlled question used to obtain missing information or a decision. |
| CTR | A document defining cost, time, resources, scope, inputs and outputs. |
| PMP | The management plan defining how the project is governed and controlled. |
| NCD | The implementation, validation and rollback plan for a controlled change. |
| FDS | The design explaining how approved requirements will be implemented and tested. |
| Baseline | The approved version of scope, schedule or cost used for comparison. |
| Milestone | A zero-duration decision or achievement such as Go / No-Go or acceptance. |
| Risk | An uncertain event that may affect scope, schedule, cost, quality or safety. |
| Issue | A problem that has already occurred and requires action. |
| Mitigation | Action taken to reduce the probability or impact of a risk. |
| Rollback | Steps used to return the system to its pre-change state. |
| Acceptance Criteria | Measurable conditions that must be met before the deliverable is approved. |
| Evidence | Screenshots, command output, logs, files or approvals proving a result. |
| RACI | A matrix defining who is Responsible, Accountable, Consulted and Informed. |
| Lessons Learned | Knowledge recorded to improve future projects and prevent repeat mistakes. |

## Suggested Student Roles

For team-based delivery, assign:

- Project Manager
- Linux Technical Lead
- Xen Orchestra / Lab Administrator
- Business Analyst / Document Controller
- Student Operations Engineer
- Test and Validation Lead
- Business Owner / Instructor

Rotate roles between projects so students experience both technical and business responsibilities.

## Suggested Assessment

| Assessment Area | Weight |
|---|---:|
| CTR, PMP, Schedule, NCD and ITQ understanding | 20% |
| Technical process implementation | 30% |
| Testing and evidence quality | 20% |
| Incident diagnosis and recovery | 15% |
| Business communication and presentation | 10% |
| Document control and project closure | 5% |

## Classroom Discussion Questions

1. Why should a student not start implementation before the Go / No-Go review?
2. What happens to project cost and schedule when the number of students or VMs changes?
3. Why is a snapshot considered a risk-control measure?
4. What is the business difference between a technical failure and an accepted risk?
5. Why must the systemd service run as `reportsvc` when the student already has root access?
6. What evidence would convince an auditor or manager that the incident was resolved?
7. How can a missing ITQ response cause rework or project delay?
8. Why is a project not complete until handover and acceptance are signed?

## Pack Contents

- `NIT_Bank_Linux_Process_Master_Document_List_REV01.xlsx`
- `NIT_Bank_Linux_Process_CTR_REV01.xlsx`
- `NIT_Bank_Linux_Process_PMP_REV01.xlsx`
- `NIT_Bank_Linux_Process_Master_Schedule_REV01.xlsx`
- `NIT_Bank_Linux_Process_NCD_Runbook_REV01.xlsx`
- `NIT_Bank_Linux_Process_ITQ_Register_REV01.xlsx`
- `NIT_Bank_Linux_Process_Functional_Design_Specification_REV01.docx`
- `NIT_Bank_Linux_Process_Functional_Design_Specification_REV01.pdf`
