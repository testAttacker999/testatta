Account Takeover Due to Improper reCAPTCHA Token Validation and Rate Limit Bypass

Summary:

I discovered that the recaptcha_token used during login can be reused and is not properly validated.

By using the same reCAPTCHA token, CSRF token, and User-Agent, I was able to perform multiple password attempts while bypassing the rate limit restriction.

Under a normal new request, an incorrect password returns a 401 response. However, when the same tokens are reused, the server returns 200 for incorrect passwords, and these attempts are not counted by the rate limit. When the correct password is provided, a 201 response is returned.

Steps to Reproduce

1. Run the attached Python PoC script.
2. The script already contains a working reCAPTCHA token, CSRF token, and User-Agent.
3. Do not change these values; only change the email address of the account on which you want to test the issue.
4. In the password.txt file, place the correct password of the target account as the last entry.
5. Observe that incorrect password attempts return 200 and are not counted by the rate limit, while the correct password results in a 201 response.

Impact

The vulnerability allows a targeted brute-force attack to be performed by bypassing the login rate limit and the intended single-use restriction of the reCAPTCHA token. If the correct password is guessed, full account takeover is possible.
2 attachments

F6694980: brute.py
F6694982: account-takeover-####.mp4



### hackerone-agent closed the report and changed the status to Duplicate / September 11, 2026, 11:42am UTC

Thank you for your submission. After reviewing your report, this issue has been previously reported and assessed in report #????. Both reports describe improper reCAPTCHA token validation on the POST /users/sessions login endpoint, where the server accepts null, invalid, empty, or reused recaptcha_token values without proper validation.

While your report provides additional details about the exploitation methodology—specifically how reusing the same reCAPTCHA token, CSRF token, and User-Agent can bypass rate limiting—this represents further exploitation detail of the same underlying CAPTCHA validation flaw identified in the original report. The core vulnerability, affected endpoint, root cause, and security impact (enabling password brute force attacks leading to account takeover) are identical.

The original report is being evaluated by the program's security team, and duplicate submissions cannot be processed separately even when they contain additional exploitation techniques.



### [I] has requested mediation from HackerOne Support.  / September 11, 2026, 11:47am UTC

I believe there may be a misunderstanding regarding the issue demonstrated in this report.

My report is not limited to accepting a null, empty, or invalid recaptcha_token. The key issue I demonstrated is that a valid, previously used reCAPTCHA token can be reused together with the same CSRF token and User-Agent to perform multiple password attempts without those attempts being counted by the login rate limit.

Specifically:

* A normal request with an incorrect password returns 401.
*  Reusing the same valid reCAPTCHA token allows subsequent incorrect password attempts to return 200.
*  These attempts are not counted by the login rate limit.
* A correct password returns 201, demonstrating a practical brute-force path to account takeover.

Could you please clarify whether report #???? demonstrated this exact token-reuse and rate-limit-bypass behavior, rather than only the acceptance of null/empty/invalid recaptcha_token values?

If #???? does not demonstrate the same exploitation path, I believe the duplicate classification should be reconsidered.

### [I] posted a comment.  /  Updated September 18, 2026, 5:27am UTC

I would like to ask for the duplicate classification of this report to be reconsidered.

When this report was closed as a duplicate of #????, the original report was still under review / marked as Needs More Info. However, the original report has now been closed as N/A.

More importantly, after reviewing the original report, I do not see evidence demonstrating the specific exploitation path described in my report: reusing the same valid reCAPTCHA token together with the same CSRF token and User-Agent to perform multiple password attempts while bypassing the login rate limit.

My report includes a working PoC and video demonstrating this specific behavior, including the fact that subsequent incorrect password attempts are not counted by the rate limit and that a correct password results in a successful login response.

Since the original report is now closed as N/A, and my report contains direct evidence of the demonstrated exploitation path and its impact, could you please reconsider the duplicate classification and reopen/review this report?

Thank you for taking another look.




`There is no answer yet`