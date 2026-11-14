# IAM-003 — Least-Privilege Security Assessment

## 1. Assessment Objective

Evaluate the Azure Role-Based Access Control (RBAC) permissions assigned to the system-assigned managed identity of the virtual machine `vm-soc-iris` within the `LAW-HuntPractice` Log Analytics workspace.

The assessment aims to identify potentially excessive permissions, evaluate associated security risks, and recommend appropriate least-privilege controls without modifying existing role assignments.

## 2. Verified Azure Role Assignments

The following role assignments were observed for the system-assigned managed identity `vm-soc-iris` in the `LAW-HuntPractice` workspace.

| Azure Role | Assignment Scope | Verification |
|---|---|---|
| Microsoft Sentinel Contributor | This resource | Confirmed |
| Microsoft Sentinel Reader | This resource | Confirmed |
| Monitoring Reader | Resource group (inherited) | Confirmed |

The Microsoft Sentinel Contributor role includes Read, Write, and Delete permissions for supported resources. This introduces a potential least-privilege concern if the managed identity only requires read-only access.

The identity's actual operational requirements have not yet been confirmed.

## 3. Security Risk Assessment

The `vm-soc-iris` managed identity has Microsoft Sentinel Contributor permissions at the `LAW-HuntPractice` workspace scope.

Azure role-definition inspection confirmed that the Contributor role includes Write and Delete operations for supported resources, including workbooks and saved searches.

If this identity is compromised, an attacker could potentially use these permissions to modify or delete security-monitoring resources, subject to applicable authorization controls.

**Risk classification:** Potential excessive privilege — requires further validation.

**Assessment limitation:** The identity's operational requirements have not been independently verified. Therefore, the Contributor assignment cannot yet be classified as unnecessary or misconfigured.

## 4. Recommended Security Controls

1. **Validate operational requirements:** Confirm whether `vm-soc-iris` requires read-only access or must modify Microsoft Sentinel resources.

2. **Apply least privilege:** If the identity only needs to view incidents and alerts, retain the appropriate Reader access and remove unnecessary Contributor permissions after testing and approval.

3. **Review inherited permissions:** Assess whether the Monitoring Reader role inherited from the resource group is necessary.

4. **Monitor privileged activity:** Review relevant Azure Activity Logs for unexpected resource modifications and role-assignment changes.

5. **Maintain change control:** Document approval, test the proposed access changes, and establish a rollback plan before modifying any assignments.

**Implementation status:** Recommendations only. No Azure permissions were modified during this assessment.

## 5. Evidence and Conclusion

### Evidence

- [IAM-003 — Redacted Managed Identity Screenshot](IAM-003-managed-identity-redacted.png)
- Azure Portal role-assignment inspection: `LAW-HuntPractice`
- Microsoft Sentinel Contributor role-definition inspection

### Conclusion

The assessment confirmed that `vm-soc-iris` has direct Microsoft Sentinel Contributor and Reader assignments, together with an inherited Monitoring Reader assignment.

Contributor permissions present a potential excessive-privilege risk if the identity's duties are read-only. Further workload validation is required before any access changes are approved.

**Assessment status:** Completed with recommendations; remediation not performed.
