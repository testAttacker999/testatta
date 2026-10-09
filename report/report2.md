Bypassing the login_id Usage Restriction by Moving the Token from the URL to a Cookie

Summary:

After successful password verification on app.#####.com, the server returns a link containing login_id, which is used to access the 2FA page.

When the same link is opened in another browser that has no active session for the account, the server does not allow the login process to continue. However, when the login_id from the same link is placed in the request cookie, the server allows access to the 2FA page without requiring the password to be entered again.

Thus, the server behaves differently when the same token is used through the URL and through a cookie, and it is possible to continue the login process through the cookie, even though reusing the same link is rejected.

Steps To Reproduce:

1. Start the login process in Browser A and enter the correct account credentials.
2. Obtain the link containing login_id from the response.
3. In Browser B, which has no active session for the account, open the same link and observe that the login process does not continue.
4. In Browser B, make the corresponding request with the same login_id placed in the request cookie.
5. Observe that the server allows access to the 2FA page without requiring the password to be entered again.
6. Enter the correct 2FA code and confirm that login succeeds.

Expected Behavior

If reuse of login_id is restricted, using the same token through the URL and through a cookie should be subject to the same validity checks.


Impact

Using login_id through a cookie makes it possible to continue a previously initiated login process without entering the password again, even when reusing the same token through the URL is rejected.
Since the token is transmitted through the URL, there may also be a risk of accidental disclosure, although this impact requires further confirmation.
1 attachment

F6948649: ####_LOGIN_ID_BYPASS.mp4


## H1 Analyst closed the report and changed the status to Informative. 