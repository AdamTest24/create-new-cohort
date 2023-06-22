# The `create-new-cohort` workflow

This workflows handles the automated setup of new cohorts for L2D. It is automatically triggered when a new issue is created labeled `"cohort"`. It is meant to work together with the [ `create-new-cohort` Issue form](https://github.com/LearnToDiscover/create-new-cohort/blob/b57afe98691946f5749b97632e9d59c429c4baa1/.github/ISSUE_TEMPLATE/new_cohort.yml)  from the same repo.

## Jobs

### `parse_issue`

Uses [`zentered/issue-forms-body-parser@v2.1.1`](https://github.com/marketplace/actions/github-issue-forms-body-parser) to parse the data fields from the Issue Form for use in the subsequent jobs. I had to add a few [`jq query commands`](https://github.com/LearnToDiscover/create-new-cohort/blob/b57afe98691946f5749b97632e9d59c429c4baa1/.github/workflows/create-new-cohort.yml#L26-L34) to get the data in the right format and store them as variables in `$GITHUB_OUTPUT`.

> **Note**:
> There might still be `Show output` and `Upload artifact` steps. These were used solely for debugging, but might still come in handy whenever somethings goes wrong.

### `invite_members`

Use the GitHub API, through `gh api` to invite instructors and students to the new cohort organisation, using the **GitHub usernames** (not emails!) specified in the Issue Form.

- A `Students` team is created to facilitate giving access to certain repos
- Instructors are added as **maintainers** of the `Students` team, this also automatically invites them to the organisation
    - An additional step is added to make instructors also **owners** of the organisation
- Students are added as **members** of the `Students`, this also automatically invites them to the organisation

> **Warning**:
> The invite steps use `continue-on-error: true`. This is because the API call will return an error for invalid usernames. However, we don't want this to fail the entire workflow, so GHA will keep running.
> The **downside** of this is that a user will have to inspect the GHA output to make sure no faulty usernames were present!

### `create_repos`

Fork/create all the necessary repos for this cohort, using the repo names specified in the Issue Form.

Lesson and assignment repos are *forked* from the **LearnToDiscover** organisation. I.e. the *requested repos should exist in the __LearnToDiscover__ organisation* before they can e forked.
An additional step makes sure that GitHub Actions are enabled for the lesson repos, so that the *sandpaper* deployments happen correctly. By default, GitHub disables GHA for forked repos.

Lastly, a new **Discussions** repo is created under the `Students` team (so that all students and instructors have access). This repo hosts the *GitHub Discussions* which act as the forum for the course. Unfortunately, [it's not possible to enable organisation-level discussions through the API](https://github.com/orgs/community/discussions/50911#discussioncomment-5420799), so this step still has to be done manually.
