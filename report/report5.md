Business logic vulnerability in the mandatory phone number verification process that allows utilizing restricted features

Summary After creating a new user on ####, the user is mandatorily redirected to the phone number verification stage. This stage is designed as a mandatory trust boundary, and without successfully completing it, the user should not be able to use the core features of the platform. During the research, it was discovered that this mandatory phone verification business process can be bypassed through response manipulation. As a result, the system allows the user to continue operating on the platform without actually completing the phone number verification.

Core of the Issue After registration, the system opens the mandatory phone number verification step. The user does not receive any legitimate option to close or bypass it. However, the client-side state can be altered by manipulating the server responses. As a result, the verification process closes, and the user is moved to a normal usage state, despite the phone number verification never being performed. This demonstrates that the system makes decisions regarding the user's state based on an incorrect point of trust. In other words, the system assumes that the verification process has closed only when the phone number verification has passed successfully. That assumption is incorrect.

Response Manipulation Example During the research, a response equivalent to the following successful response was used:


JSON
{
"data": {
"setPhoneNumberV2": {
"status": "SUCCESS",
"message": "Verification code sent successfully.",
"__typename": "SetPhoneNumberPayload"
}
}
}

Based on this response, the client side considers that the process has successfully completed and removes the mandatory verification window, even though no actual verification took place.

Steps to Reproduce

Create a new #### user.
Complete the registration process.
Reach the mandatory phone number verification page.
Intercept the requests and responses of the verification process.
Modify the corresponding response.
Observe that the verification process closes.
The user becomes usable without phone number verification.
Confirmed Results During the research, it was confirmed that a user who has not passed phone number verification is able to:
Access the main platform.
Open sellers' and live stream pages.
Follow other users.
Join live streams.
Appear in the viewer list.
Be counted as a live stream participant.
Edit their personal information.
Add an address.
Add a payment method.



Live Stream Observations After joining a live stream, the system displays a message stating that phone number verification is required for certain actions. Nevertheless, the user is already considered a live stream participant. During the research, it was confirmed that:
The user appears in the live stream.
The join event is registered in the chat.
The view counter counts the user as a present viewer.
This shows that after the bypass, the user is not completely blocked, but rather participates in the core business processes of the platform.
Payment Process Observations The payment process was tested during the research. A bank card was added to the user who had not passed phone number verification. Subsequently, a payment attempt was made. The system transmitted the transaction request to the banking system. The proof of this is that a corresponding notification was received from the bank. The payment was not completed solely due to the lack of sufficient funds on the account. However, the experiment shows that after the bypass, the user reaches the business logic associated with payments.

Bypassing the Phone Number Limit During the research, it was also confirmed that under normal conditions, #### limits the number of users associated with a single phone number. According to the observed behavior, a maximum of three users can be created with one phone number. Thanks to this vulnerability, that limitation is completely bypassed. Since phone number verification is no longer necessary, an attacker can create a large number of users without providing new phone numbers. This increases the impact of the vulnerability, as those users continue to participate in the platform's business processes, including:
Following sellers.
Participating in live streams.
Affecting view metrics.
Affecting engagement metrics.

Postscript: Evidence of Transaction Attempt
Note regarding the attached image (photo_2026-06-01_18-57-41.jpg):
The attached screenshot is a notification from my local Armenian bank (Evocabank/ARCA) regarding the transaction attempt performed during the video PoC. Even though the transaction did not complete due to insufficient funds, the fact that the request reached the banking system proves that the phone verification bypass is successful and grants access to the payment business logic.
The text in the screenshot is in Armenian, written using Latin characters. Here is the translation for clarity:
"#### ####" translates to "Insufficient Funds".
"####" translates to "Balance".
Merchant: ####, US
Amount: 5.45 USD
Date/Time: 01.06.2026 07:28 (Matches the time of the recorded PoC).
This evidence confirms that an unverified user can bypass the mandatory verification wall and trigger actual financial transactions on the platform.

Cause The system trusts the state change of the verification process instead of verifying on the server side during each action whether the user is actually verified or not. Due to this misplaced trust, the phone number verification bypass leads to bypassing business restrictions.

Impact

Impact This vulnerability violates ####'s mandatory trust model. Phone number verification is designed as a prerequisite for certain actions, but that state can be bypassed. As a result, an attacker can:
Create a large number of unverified users.
Perform follow manipulation.
Affect engagement metrics.
Participate in the live stream environment.
Reach payment-related processes.
Utilize system features without the intended establishment of trust.
2 attachments

F6020219: photo_2026-06-01_18-57-41.jpg
F6020223: ######.mp4


### H1 Analyst changed the scope from www.####.com to com.####_mobile. / June 2, 2026, 5:13am UTC

### H1 Analyst closed the report and changed the status to Duplicate / June 2, 2026, 5:13am UTC


Thank you for your submission. After reviewing your report, this issue has been previously reported and assessed in report #?????. Both reports describe a phone number verification bypass that allows unverified accounts to access restricted platform features including livestreams, payment methods, and user interactions.
While your report demonstrates response manipulation of the setPhoneNumberV2 GraphQL mutation, and the original report may have used a different exploitation technique, both vulnerabilities exploit the same fundamental security weakness: the lack of proper server-side enforcement of phone verification requirements before granting access to restricted features.
The original report was submitted earlier and identified the same phone verification bypass vulnerability affecting the same component with identical security impact. According to this program's duplicate policy, different exploitation methods of the same underlying vulnerability are considered duplicates when they target the same security control and achieve the same outcome.
Thanks,



### [I] posted a comment. / June 2, 2026, 10:16am UTC

Hello Team,
Thank you for reviewing the report and for the clarification regarding the duplicate determination.
I completely understand the reasoning regarding different exploitation methods leading to the same underlying security weakness and I respect the duplicate decision if the previously reported issue indeed covers the same vulnerability.
However, I would appreciate clarification regarding one point.
My research and proof-of-concept were performed through the web application (www.####.com), and all observations, testing, and demonstrated impacts were reproduced within the web environment.
I noticed that during triage the report scope was changed from www.####.com to com.####_mobile.
Could you please clarify whether the original duplicate report (#????) specifically covered the same web-based phone verification bypass and the same server-side trust boundary issue, or whether the duplicate mapping was based on a broader platform-level assessment?
I am not disputing the duplicate decision itself; I simply want to better understand how the issue was categorized so I can improve future reports and avoid reporting already known variants.
Thank you for your time and clarification.
Best regards,


`There is no answer yet`