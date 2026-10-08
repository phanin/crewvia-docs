# User management

Company Admins manage the people in their own company from **Users** in the
sidebar (`/users/`). Nobody else can open these pages.

## What an Admin can do

| Action | Notes |
|---|---|
| List users | Shows name, username, email, role and status for the Admin's company only |
| Add a user | Username, name, email, phone, one role, and an initial password that must pass the password rules |
| Edit a user | Name, email, phone and role. The username does not change. |
| Deactivate or reactivate | A deactivated user cannot sign in, and an open session ends on the next request |

Users are never deleted. Deactivating keeps their history, and the audit fields
on other records keep pointing at a real person.

## Rules

| Rule | Reason |
|---|---|
| Only the `core.users_manage` permission opens these pages (Admin role) | Least privilege |
| A user sees and edits only their own company's users. Another company's user returns "not found". | Tenant isolation |
| Platform superusers have no company, so they are refused here and use `/admin/` | They are not part of any tenant |
| The company cannot be chosen in the form. It is always the Admin's company. | A posted value cannot move a user to another tenant |
| Each user has exactly one role | Simple, predictable menus |
| Email is required and unique (case-insensitive) | Password reset must reach one person |
| An Admin cannot change their own role or deactivate themselves | Prevents lock-out |
| The last active Admin cannot be demoted or deactivated | A company always keeps an Admin |
| Deactivate and reactivate are POST requests | A link or a prefetch cannot change access |

## Where it lives

| Piece | File |
|---|---|
| Views | `apps/accounts/views.py` |
| Forms | `apps/accounts/forms.py` |
| Last-Admin rule | `apps/accounts/services.py` |
| URLs | `apps/accounts/user_urls.py` (namespace `users`) |
| Pages | `templates/accounts/user_list.html`, `user_form.html` |
| Tests | `apps/accounts/tests/test_user_management.py` |

## Known limits

- The duplicate-email message does not say which company the email belongs to,
  but it does confirm the address is already in use somewhere on the platform.
- There is no invitation email yet. The Admin sets the first password and shares
  it. Users change it with the password reset flow.
- No bulk actions, search or pagination. A single company is small enough for now.
