# grafana-git-sync-demo

Demostration on how to use [Grafana Git Sync](https://grafana.com/docs/grafana/latest/observability-as-code/provision-resources/git-sync-setup/) and [grafanactl](https://github.com/grafana/grafanactl).

## Requirements

- Have terminal :).
- Have image rendering running and configured. See [Image Rendering documentation](https://grafana.com/docs/grafana/latest/setup-grafana/image-rendering/).
- Grafana nightly build running and configured to enable Git Sync as described [Git Sync documentation](https://grafana.com/docs/grafana/latest/observability-as-code/provision-resources/git-sync-setup/). Make sure you set up webhooks and image rendering so that pull request comments and instaneous pulling work properly.
- Have a repository to push / pull changes to.
- Have a PAT token with correct permissions for that repository including webhook events and pull requests. See [Create a Github Access Token](https://grafana.com/docs/grafana/latest/observability-as-code/provision-resources/git-sync-setup/#create-a-github-access-token)
- Have some dashboards and folders already created in the system.
- Install [grafanactl](https://grafana.github.io/grafanactl/installation/)

## Basic workflow

### 1) Migrate Dashboards to Github

Here it's how you can migrate your existing dashboards to GitHub and enable bi-directional synchronization between Grafana and Github:

1. In Grafana:
    1. Go to `Dashboards` and show the existing dashboards and folders. 
1. In Github:
    1. Go to the repository in `github.com` to display the contents.
1. In Grafana, go to `TODO/provisioning` to configure your first repository:
    1. Paste your PAT token and input your repository details. Click Next.
    1. Grafana will now tell you how many dashboards are in your instance and if you want to migrate them to Github. 
    1. Select that option and initiate the migration.
    1. Wait till migration completed successfully and it will tell how many dashboards were migrated.
    1. Enable the option `TODO` to enable dashboards previews screenshots on pull requests.
    1. Clik in `Finish`.
1. In Github:
    1. Refresh your repository content to see a new folder `/grafana` containing the dashboards.
    1. Open one dashboards to show briefly the content.
1. In Grafana:
    1. Open the Github connection by clicking in `TODO`.
    1. Show the repository status page (e.g. how many dashboards, recent jobs, resources tab and files tab).

That's how you migrate to Git Sync and create a Github connection with Grafana! Git Sync will keep grafana always in sync with Github.

### 2) Use Pull Requests for your dashboards

Here it's how you can edit dashboards in Grafana and submit them as Pull Requests in Github before they are deployed: 
1. In Grafana:
    1. Go to `Dashboards` and open a dashboard to see that they have now the provisioned purple badge `<->`.
    1. Open one of the dashboards
    1. Edit it to change something. 
    1. Click on save and see that now we have new special drawer for this provisioned dashboards.
    1. Write a commit message. 
    1. Click on "TODO" to create a branch for the changes.
    1. The UI will redirect you to dashboards page and show a banner telling you that this is a preview and it doesn't exist yet in Grafana. That banner will offer you a button `Open Pull Request`.
    1. Click in `Open Pull Request`.
    1. You will be redirected to your repository in `github.com` to open the pull request.
1. In Github:
    1. Open the pull request.
    1. Wait some seconds until our Grafana integration makes a comment on the pull request with previews of the before and after of your changes in this dashboard. 
    1. Merge the pull request.
1. In Grafana, 
    1. Go to your dashboard (or refresh) to see the changes applied automatically. 

### 3) Edit a dashboard using `grafanactl`

Here it's how you can edit dashboards using `grafanactl` and see the changes reflected in both Github and Grafana:
In your terminal: 
... 

