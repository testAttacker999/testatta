Title Previously Issued challenge_tokens Are Not Revoked After Password Change

On app.#####.com, during a 2FA login, the challenge_token created after successful password verification remains valid even after the account password is changed.

After changing the password, active sessions are revoked, but the challenge_tokens obtained using the previous password are not revoked. As a result, they can be used to initiate the 2FA login process and, with the correct 2FA code, access the account without knowing the new password.

I also confirmed that before changing the password, it is possible to obtain multiple separate challenge_tokens for the same account, and each of them continues to work after the password is changed.

Steps To Reproduce

1. Enable 2FA on the account.
2. Perform a login request for the account and obtain a challenge_token. Repeat the process several times and save the obtained challenge_tokens.
3. Change the password from within the account.
4. Try to use each of the challenge_tokens obtained before the password change.
5. Enter the correct 2FA code.
6. Login is successful without providing the new password.

Expected Behavior
After a password change, previously issued and unused challenge_tokens should be revoked.

Impact
Changing the password does not revoke the challenge_tokens issued before the password change. Therefore, a person who previously obtained such a token can continue the login process and, with the account's 2FA code, access the account without knowing the new password.
1 attachment

F6941220: #####_challenge_token.mp4





## H1 Analyst closed the report and changed the status to Informative. 

Thank you for your submission!
Although your finding might appear to be a security vulnerability, this behavior does not really pose a concrete and exploitable risk to the platform.
Your effort is nonetheless appreciated and we wish that you'll continue to research and submit any future security issues you find.
Kind regards,