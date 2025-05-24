# grafana-git-sync-demo

> [!WARNING]
> Git Sync is currently in an experimental phase and may have limitations or breaking changes. While it represents Grafana's first step toward comprehensive Observability as Code, we don't recommend using it in production or critical environments.

Hey there! This is a fun demo showing how to use [Grafana Git Sync](https://grafana.com/docs/grafana/latest/observability-as-code/provision-resources/git-sync-setup/) and [grafanactl](https://github.com/grafana/grafanactl).

## Target Audience

This demo is designed for:
- **DevOps Engineers** looking to implement Observability as Code practices.
- **SREs** wanting to version control their monitoring dashboards.
- **Grafana Users** interested in automating dashboard management.
- **Teams** seeking to improve collaboration on dashboard development.
- **Anyone** curious about Grafana's Git Sync feature and its capabilities.

## Requirements

- A terminal (you've got this!).
- Image rendering up and running. Check out the [Image Rendering documentation](https://grafana.com/docs/grafana/latest/setup-grafana/image-rendering/).
- Grafana nightly build with Git Sync enabled. Follow the [Git Sync documentation](https://grafana.com/docs/grafana/latest/observability-as-code/provision-resources/git-sync-setup/) and make sure webhooks and image rendering are set up so pull request comments and instant sync work smoothly.
- An empty repository to push and pull changes.
- A PAT (Personal Access Token) with the right permissions for your repository, including webhook events and pull requests. See [Create a Github Access Token](https://grafana.com/docs/grafana/latest/observability-as-code/provision-resources/git-sync-setup/#create-a-github-access-token).
- Some dashboards and folders already created in your Grafana instance.
- [grafanactl](https://grafana.github.io/grafanactl/installation/) installed.

## Intro


[Git Sync](https://grafana.com/docs/grafana/latest/observability-as-code/provision-resources/) is Grafana's solution for implementing [Observability as Code (OaC)](https://grafana.com/docs/grafana/latest/observability-as-code/). It enables bi-directional synchronization between your Grafana dashboards and a Git repository, allowing you to:

- Version control your dashboards and other Grafana resources
- Review changes through pull requests with visual previews
- Automate dashboard deployments
- Collaborate with team members using familiar Git workflows
- Maintain a single source of truth for your observability stack

This demo will walk you through setting up Git Sync and using it to manage your Grafana dashboards effectively.

## Step 1: Migrate Dashboards to GitHub

Here's how to migrate your existing dashboards to GitHub and enable bi-directional sync between Grafana and GitHub:

1. In Grafana:
    1. Go to `Dashboards` and check out your existing dashboards and folders.
1. In GitHub:
    1. Open your repository on `github.com` to see what's inside.
1. In Grafana, go to `Administration/provisioning` to set up your first repository:
    1. Click in `Configure Git Sync`.
    1. Paste your PAT token and enter your repository details. Click `Choose what to synchronize`.
    1. Grafana will show you how many dashboards are in your instance and ask if you want to migrate them to GitHub.
    1. Select the migration option to sync all dashboards `Sync all resources from external storage` and start the process by click in `Synchronize with external storage` and then `Begin synchronization`.
    1. Wait for the migration to complete. Grafana will tell you how many dashboards were successfully migrated.
    1. Click on `Choose additional settings`.
    1. Enable the `Enable dashboards previews in pull requests` option to have dashboard preview screenshots on pull requests.
    1. Click `Finish`.
1. In GitHub:
    1. Refresh your repository to see a new `grafana` directory containing your migrated dashboards.
    1. Look at the folder structure and files.
    1. Open a dashboard file to peeak at its contents.
1. In Grafana:
    1. Open the GitHub connection by clicking `View`.
    1. Check out the repository status page (e.g., dashboard count, recent jobs, resources tab, and files tab).

That's it! Your Grafana instance is now connected to GitHub with Git Sync, keeping everything in sync.

## Step 2: Use Pull Requests for Your Dashboards

You can edit dashboards in Grafana and submit changes as pull requests in GitHub before they go live:

1. In Grafana:
    1. Go to `Dashboards` and open a dashboard. Provisioned dashboards will have a purple `<->` badge.
    1. Open a dashboard.
    1. Make and save your changes. A new drawer will pop up for provisioned dashboards with new saving options.
    1. Write a commit message.
    1. Click "TODO" to create a branch for your changes.
    1. You will be redirected to the dashboards page and see a banner indicating this is a preview and not yet live in Grafana. 
    1. The banner will offer an `Open Pull Request` button, click on it.
    1. A new tab will be opened for you to open a pull request in Github.
1. In GitHub:
    1. Open the pull request.
    1. Wait a few seconds for the Grafana integration to comment on the pull request with before-and-after previews of your dashboard changes.
    1. Enjoy the view of the side-to-side comparison.
    1. Merge the pull request.
1. In Grafana:
    1. Go to your dashboard (or refresh) to see the changes were applied automatically.

## Step 3: Edit a Dashboard from Github

1. In Github, 
    1. Go to the dashboard you previously modified (or another one). 
    1. Edit the title of a panel (e.g. "Modified From Github"). 
    1. Save and push to the branch. 
1. In Grafana, 
    1. Go to that dashboard (or refresh).
    1. See that changes are now reflected.

## Step 4: Edit a Dashboard Using `grafanactl`

> `grafanactl` is a command-line tool that helps you manage Grafana resources like dashboards, folders, and datasources. The tool integrates with Git Sync, allowing you to manage your dashboards through both the Grafana UI and command line, giving you flexibility in how you work with your dashboards.

Here is how you can edit dashboards using `grafanactl` and see the changes reflected in both GitHub and Grafana:

1. In your terminal:
    1. First, explore the available commands:
        ```bash
        grafanactl -h
        Usage:
        grafanactl [command]

        Available Commands:
        completion  Generate the autocompletion script for the specified shell
        config      View or manipulate configuration settings
        help        Help about any command
        resources   Manipulate Grafana resources

        Flags:
        -h, --help            help for grafanactl
            --no-color        Disable color output
        -v, --verbose count   Verbose mode. Multiple -v options increase the verbosity (maximum: 3).
            --version         version for grafanactl

        Use "grafanactl [command] --help" for more information about a command.

        ```
        This will show you all available options and commands for the CLI tool.

    1. Pull the latest resources from Grafana:
        ```bash
        grafanactl resources pull
        ```
        This command synchronizes your local environment with your Grafana instance, downloading all dashboards and other resources.

    1. List all available dashboards:
        ```bash
        grafanactl resources get dashboards
        ```
        This will display a list of all dashboards with their IDs, which you'll need for editing.

    1. Edit a specific dashboard:
        ```bash
        grafanactl resource edit dashboard <ID>
        ```
        Replace `<ID>` with the actual dashboard ID from the previous step. This will open your default editor where you can make changes to the dashboard JSON.

    1. Push your changes back to Grafana and GitHub:
        ```bash
        grafanactl resources push
        ```
        This command will sync your local changes with both Grafana and your GitHub repository.

1. In GitHub:
    1. Navigate to your repository
    1. Check the commit history to see your dashboard changes
    1. Review the changes in the dashboard JSON file

1. In Grafana:
    1. Open your dashboards section
    1. Verify that your changes have been automatically synchronized


---

🎉 Excellent work! You've now mastered Git Sync for Grafana dashboards and can confidently manage your dashboards through multiple interfaces - the Grafana UI, GitHub, and the command line. This setup enables collaborative dashboard development with proper version control and review processes. 🎉

Thank you for following along with this guide. You're now part of an elite group who keep their dashboards version-controlled and collaborative. Happy dashboarding! 🚀

