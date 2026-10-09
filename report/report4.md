Broken Recovery Flow Allowing External Control Over User Login State

## April 22, 2026, 10:44am UTC

Summary
The application contains a business logic vulnerability resulting from an improper interaction between the account-level lockout mechanism and the password reset (recovery flow).
Testing has confirmed that an attacker can force a target account into a lockout state through failed login attempts. More critically, the password reset mechanism does not restore a clean authentication state.
As a result, an external actor can continuously influence the account's authentication state and repeatedly prevent successful login attempts, even after a legitimate password reset.

Core Issue
The vulnerability is caused by the following technical behaviors:
Account-Level Tracking: Failed login attempts are tracked at the account level rather than per IP or device.
Source-Independent Lockout: The lockout is triggered regardless of the request origin.
State Persistence: Password reset does not reset or invalidate lockout-related counters or states.
Missing Recovery Window: There is no grace period or protected window after a password reset.
This combination allows an attacker to continuously re-trigger lockout conditions, even immediately after a successful recovery.

Steps to Reproduce

1. Perform multiple failed login attempts against a target account (e.g., via a script using multiple IP addresses).
2. Observe that the account enters a lockout state requiring a password reset.
3. Perform a password reset (legitimate recovery process).
4. Attempt to log in with valid credentials.
5. Simultaneously continue sending failed login attempts.
6. Observe that the account quickly returns to a locked/restricted state.
7. This behavior can be maintained indefinitely, effectively preventing successful authentication.


Key Observation
The password reset does not restore a reliable authentication state. There is no protected window in which a legitimate user can securely log in after recovery. The lockout mechanism remains externally influenceable, allowing continuous disruption of access.


Additional Technical Note
This is not expected lockout behavior because:
The protection mechanism can be externally controlled.
The recovery flow does not provide isolation or a state reset.
The authentication state remains non-deterministic after a password reset.
This indicates a flaw in authentication state management rather than intended security behavior.

Suggested Mitigations
Introduce a grace period during which lockout cannot be immediately re-triggered.
Bind failed attempts to IP, device, or behavioral signals instead of the account only.
Implement user notifications for suspicious login activity.
Provide alternative authentication methods (OTP, recovery codes).
Consider adaptive or source-based rate limiting instead of strict account-level lockout.

Impact
This vulnerability allows an attacker to:
Continuously prevent a user from successfully logging into their account.
Maintain persistent disruption of the authentication process.
Externally control when access is possible or blocked.
This effectively results in a targeted denial of access at the account level. Given the system's use in professional, medical, or organizational contexts, this may lead to:
Disruption of critical workflows.
Loss of access to operational systems.
Delays in time-sensitive actions.
2 attachments

F5778478: exploit1.py
F5778630: ####.mp4


### h1 analyst closed the report and changed the status to Duplicate / June 11, 2026, 7:22am UTC

Thank you for your report!
We noticed that this issue was already submitted previously by you. No additional information was provided to demonstrate the uniqueness of this report against the earlier report. As a result, we will be closing this report as a duplicate.
If you are not satisfied with the closure status of the earlier report or if responses to your questions are delayed, please feel free to raise a mediation request on the report.
We appreciate the time and efforts you have invested in reporting this. Thank you for your understanding.
Have a great day ahead! 
Best regards,


### [I] posted a comment. / June 11, 2026, 8:11am UTC

Thank you for your response.
However, I would like to clarify why this report is not a duplicate. In the previous report, the issue was limited to a one-time access scenario where the user could regain control of the account simply by resetting their password.
In this new report, I am demonstrating a significantly more severe issue: the password recovery flow is completely broken, and resetting the password no longer restores access to the account. This indicates that the underlying vulnerability has evolved or is fundamentally different from the one previously reported.
Could you please re-evaluate the report based on this new finding? I believe this distinction is critical for the security of the affected accounts.
Best regards,


### [I] has requested mediation from HackerOne Support.  / September 3, 2026, 9:10am UTC


Hello HackerOne Mediation Team,
I would like to request mediation regarding the Duplicate closure of this report.
I understand that this report is related to the account lockout report I previously submitted. However, I believe that this report demonstrates a separate and significantly different security condition that was not demonstrated in the previous report.
In my previous report, I showed that an attacker could force a user's account into a lockout state. However, after the password reset process, the legitimate user could restore access and use the account again.
In this report, I demonstrated a more specific issue: the password recovery process does not restore the account's authentication state in a way that causes the attacker's influence to stop.
Specifically:
The attacker causes an account lockout by making multiple incorrect login attempts.
The user performs a password reset, expecting to be able to log in again after completing the recovery process.
The password reset is successfully completed, but the attacker's influence over the lockout state does not stop.
The attacker can continue sending incorrect authentication attempts.
As a result, the account can enter a lockout state again, allowing the attacker to continuously prevent the legitimate user from accessing the account even after the password reset.
Therefore, the difference is not only in the impact or severity.
The previous report demonstrated the ability to cause an account lockout, whereas this report demonstrates that the password recovery mechanism does not neutralize the attacker's continued influence over the authentication state.
In my opinion, this is an important distinction because after a password reset, the system should return the user to a functional authentication state. In this case, the recovery process does not ensure this, and the attacker can continue to control the account's accessibility by causing another lockout.
Therefore, I kindly request that you reconsider the Duplicate classification of this report, taking into account this technical difference from the previous report.
Thank you for reviewing the report.


`There is no answer yet`