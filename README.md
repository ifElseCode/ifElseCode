## Branching Strategy for Collaborative Development

This document outlines a branching strategy for effective collaboration, code quality, and deployment on GitHub.

### Branch Structure

- **main Branch:**
  - Contains production-ready, thoroughly tested code.
  - Only updated by merged pull requests (no direct commits).
- **staging Branch:**
  - Acts as a testing environment before code goes to main.
  - Features are merged from feature/hotfix branches for integration and testing.
  - Periodically merged into main after successful testing.
- **Feature Branches (e.g., feature/add-login, feature/ui-update):**
  - Each new feature or task gets its own branch, created from the latest staging.
  - Developers work and push code to their feature branches.
  - Merged into staging via pull requests.
- **Hotfix Branches (e.g., hotfix/fix-login-bug):**
  - Created directly from main for urgent fixes.
  - Merged back to both main and staging after resolving the issue.

### Workflow

1. **Setting Up the Repository**

   - Create the repository on GitHub.
   - Set `main` as the default branch.
   - Create the `staging` branch:

   ```bash
   git checkout main
   git checkout -b staging
   git push -u origin staging
   ```

2. **Protect Branches**

   - Go to repository settings on GitHub > Branches > Branch protection rules.
   - Protect `main` and `staging` branches:
     - Require pull request reviews before merging.
     - Require status checks to pass before merging (e.g., automated tests).
     - Restrict who can push directly to the branch.

3. **Development Workflow**

   a) **Create a Feature Branch:**

   ```bash
   git checkout staging
   git pull origin staging
   git checkout -b feature/add-login
   ```

   - Work on the feature and commit changes.
   - Push the branch to remote repository.

   b) **Create a Pull Request**

   - Open a pull request on GitHub from `feature/add-login` to `staging`.
   - Request code reviews.

   c) **Merge into staging:**

   - After approval and passing tests, merge the feature branch.

   d) **Testing in staging:**

   - Deploy the `staging` branch for QA in a testing environment.
   - Fix bugs by creating new branches from `staging`.

   e) **Merge into main:**

   - Once testing is successful, create a pull request from `staging` to `main`.
   - Merge the pull request and deploy to production.

### Branching Diagram

```plaintext
main
 │
 └─── staging
       │
       ├─── feature/add-login      (PR → staging)
       │
       ├─── feature/ui-update      (PR → staging)
       │
       └─── hotfix/fix-login-bug   (PR → main & staging)

```
### Best Practices

- **Descriptive Branch Names:**
  - `feature/` for new features (e.g., `feature/user-auth`).
  - `hotfix/` for urgent fixes (e.g., `hotfix/payment-bug`).
  - `release/` for release-specific changes (e.g., `release/v1.0`).
- **Automate CI/CD:** Use GitHub Actions or other CI/CD tools for automated testing on pull requests.
- **Pull Requests:** Require at least one reviewer for merging into staging and main. Use meaningful descriptions and link tasks/issues.
- **Code Reviews:** Enforce code reviews to maintain code quality. Define guidelines for consistent coding practices.
- **Release Tags:** Use Git tags to mark production releases (e.g., `v1.0.0`).
- **Documentation:** Maintain clear guidelines in a `CONTRIBUTING.md` file.

This branching strategy promotes a clean and organized workflow, minimizes conflicts, and facilitates a smooth development to production transition.


## Branch Protection Rules

To maintain a clean and stable development process, it's essential to set branch protection rules on the `main` and `staging` branches in your GitHub repository. Below are the recommended rules for each branch.

---

### **Branch Protection Rules for `main` Branch**

The `main` branch represents production-ready code, so its integrity is critical. Here are the recommended rules:

1. **Require Pull Request Reviews**:
   - Enable "Require a pull request before merging."
   - Require at least **one or two reviewers** to approve the pull request before merging.
   - Dismiss stale pull request approvals when new changes are pushed.

2. **Require Status Checks**:
   - Enable "Require status checks to pass before merging."
   - Add specific CI/CD checks (e.g., tests, build verification).
   - Ensure the branch is up to date before merging (to avoid merge conflicts).

3. **Restrict Pushes**:
   - Only allow users with administrative privileges to push directly to `main`.
   - Encourage all contributions through pull requests.

4. **Require Signed Commits** *(Optional)*:
   - Enable "Require signed commits" to ensure all commits are verified and made by authorized contributors.

5. **Prevent Force Pushes**:
   - Disallow force-pushing to the branch to prevent accidental overwrites of history.

6. **Restrict Branch Deletion**:
   - Add rules to prevent accidental deletion of the `main` branch.

---

### **Branch Protection Rules for `staging` Branch**

The `staging` branch is a testing ground for code changes, but it also needs to be protected to ensure its stability. 

1. **Require Pull Request Reviews**:
   - Enable "Require a pull request before merging."
   - Require at least **one reviewer** to approve the pull request.
   - Dismiss stale approvals when new changes are pushed.

2. **Require Status Checks**:
   - Enable "Require status checks to pass before merging."
   - Add CI/CD checks (e.g., integration tests, linting).
   - Optionally, require the branch to be up to date with `staging` before merging.

3. **Restrict Pushes**:
   - Only allow team leads or specific roles to push directly to `staging`.

4. **Prevent Force Pushes**:
   - Disallow force-pushing to maintain a clean commit history.

5. **Require Linear History** *(Optional)*:
   - Require a linear commit history to avoid merge commits and ensure clarity.

---

### **How to Configure Branch Protection Rules in GitHub**

1. Go to your repository on GitHub.
2. Navigate to **Settings** > **Branches** > **Branch Protection Rules**.
3. Add rules for `main` and `staging`:
   - Enter the branch name (e.g., `main` or `staging`).
   - Enable the desired protection options as described above.
4. Save the rules.

---

### **Summary of Key Rules**

| **Rule**                         | `main`       | `staging`    |
|-----------------------------------|--------------|--------------|
| Require Pull Request Reviews      | ✔            | ✔            |
| Require Status Checks             | ✔            | ✔            |
| Prevent Direct Pushes             | ✔            | ✔            |
| Prevent Force Pushes              | ✔            | ✔            |
| Require Linear History (Optional) | ✔            | Optional     |
| Require Signed Commits (Optional) | ✔            | Optional     |
| Dismiss Stale Pull Request Approvals | ✔         | ✔            |

---

These rules ensure a robust workflow, prevent accidental errors, and promote collaboration in your development process.
