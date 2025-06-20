Bug #1: Page fails to load due to invalid protocol
    Summary: Registration page link uses h4p:// instead of http://
    Steps to Reproduce:
        1. In browser address bar, enter h4p://18.216.201.86/
        2. Press Enter
    Actual Result: Browser error “This site can’t be reached”
    Expected Result: Page loads successfully at http://18.216.201.86/
    Priority: Critical
    Notes: Typo in link breaks access for all users.

Bug #2: No client-side email format validation
    Summary: Invalid email formats are accepted by the form
    Steps to Reproduce:
        1. Navigate to http://18.216.201.86/
        2. Enter valid username/password, but set email=user@@domain
        3. Submit form
    Actual Result: Form submits; backend rejects with generic error or accepts malformed data
    Expected Result: Inline validation prevents submission and shows “Enter a valid email address.”
    Priority: Medium
    Attachments: Screenshot of console/network log if backend error returned.

Bug #3: Password strength not enforced
    Summary: Passwords shorter than 8 characters are accepted
    Steps to Reproduce:
        1.Load registration page
        2. Enter username=test, password=12345, email=a@b.com
        3. Submit form
    Actual Result: Registration completes successfully
    Expected Result: Inline error “Password must be at least 8 characters.”
    Priority: High
