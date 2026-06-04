# System Test Scenarios – Security

## Test Case 1: Unauthorized Unlock Attempt

### Precondition
- Smart lock is registered
- User is NOT authenticated

---

### Steps
1. Send unlock request with invalid token
2. System processes request

---

### Expected Result
- Unlock command is rejected
- Door remains locked
- Security alert is generated
- Event is logged

---

### Status
- To be tested