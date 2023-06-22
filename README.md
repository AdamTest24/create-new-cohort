# Creating a new cohort

This repository hosts the [Issue Form](https://github.com/LearnToDiscover/create-new-cohort/issues/new/choose) and associated [GitHub Actions workflow](.github/workflows/) to automate as many of the steps as possible when creating a new cohort.

## Steps

Unfortunately, not every step of the process can be automated. Therefore, an overview is provided here of the steps to take:

1. Make sure you are logged in to the **L2D-admin** account
2. [Create a new organization](https://github.com/account/organizations/new?plan=free) for the cohort
3. Fill out the [Issue Form](https://github.com/LearnToDiscover/create-new-cohort/issues/new/choose) in the `LearnToDiscover/create-new-cohort`  repository with all the relevant information for the new cohort
4. Submit the issue, this should launch a [GitHub Actions workflow](https://github.com/LearnToDiscover/create-new-cohort/actions) with the name that you used as the title in the Issue Form. Check whether the workflow has launched and wait until it ha completed (should only take a couple of seconds)
5. Go to the organization you created in **step 2**, if everything went well, all the requested repositories should be there and [member invitations](https://docs.github.com/en/organizations/managing-membership-in-your-organization/canceling-or-editing-an-invitation-to-join-your-organization) should have been sent out
6. [Enable organization-level Discussions](https://docs.github.com/en/organizations/managing-organization-settings/enabling-or-disabling-github-discussions-for-an-organization) for the new cohort organization and choose the **Discussions** repo to host these
7. Go to the forked lesson (sandpaper) repositories, and, so that the lessons are deployed automatically: go to the `Actions` tab of the repository and click on the green button
  ![Pasted image 20230525140731](https://github.com/LearnToDiscover/create-new-cohort/assets/38256462/7f6bad5d-17bd-4706-9855-f8a2178df2e3)

8. Set up the [GitHub Classroom](https://classroom.github.com/classrooms/new) using the organization you created in **step 2**

## Permissions

Note that some of the steps in the GHA workflow requires **sufficient permissions**, to allow forking and creating new repositories. To this end, we recommend using an `admin` account and set up a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#personal-access-tokens-classic) for this account with at least the following permissions enabled:

![permissions-screenshot](https://github.com/LearnToDiscover/create-new-cohort/assets/38256462/d5da8cab-ace6-4c79-8488-62fc4fae49ab)

Next, [add this PAT as a **secret** in the settings of _this_ repository](https://github.com/LearnToDiscover/create-new-cohort/settings/secrets/actions)

