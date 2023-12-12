# bonjour
Apprendre le français

Steps to create this site:

1. initialize
    ```sh
    hugo new site bonjour
    cd bonjour
    git init
    git add .
    git commit -m 'upload bonjour'
    git remote add origin https://github.com/lijqhs/bonjour.git
    git pull origin main
    git config pull.rebase true
    git pull origin main
    git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
    git submodule update --init --recursive
    ```

2. configure hugo

   - delete `hugo.toml`, copy `config.yaml` from [PaperMod exampleSite](https://github.com/adityatelange/hugo-PaperMod/blob/exampleSite/config.yml).
   - modify configurations in `config.yaml`

3. configure github

    - push repo to GitHub
    - modify repo setting to `GitHub Actions`
    - add `.github/workflows/hugo.yaml`, as in [Host on GitHub Pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
        ```yaml
        # Sample workflow for building and deploying a Hugo site to GitHub Pages
        name: Deploy Hugo site to Pages

        on:
        # Runs on pushes targeting the default branch
        push:
            branches:
            - main

        # Allows you to run this workflow manually from the Actions tab
        workflow_dispatch:

        # Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
        permissions:
        contents: read
        pages: write
        id-token: write

        # Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
        # However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
        concurrency:
        group: "pages"
        cancel-in-progress: false

        # Default to bash
        defaults:
        run:
            shell: bash

        jobs:
        # Build job
        build:
            runs-on: ubuntu-latest
            env:
            HUGO_VERSION: 0.121.0
            steps:
            - name: Install Hugo CLI
                run: |
                wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
                && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
            - name: Install Dart Sass
                run: sudo snap install dart-sass
            - name: Checkout
                uses: actions/checkout@v4
                with:
                submodules: recursive
                fetch-depth: 0
            - name: Setup Pages
                id: pages
                uses: actions/configure-pages@v4
            - name: Install Node.js dependencies
                run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
            - name: Build with Hugo
                env:
                # For maximum backward compatibility with Hugo modules
                HUGO_ENVIRONMENT: production
                HUGO_ENV: production
                run: |
                hugo \
                    --gc \
                    --minify \
                    --baseURL "${{ steps.pages.outputs.base_url }}/"          
            - name: Upload artifact
                uses: actions/upload-pages-artifact@v2
                with:
                path: ./public

        # Deployment job
        deploy:
            environment:
            name: github-pages
            url: ${{ steps.deployment.outputs.page_url }}
            runs-on: ubuntu-latest
            needs: build
            steps:
            - name: Deploy to GitHub Pages
                id: deployment
                uses: actions/deploy-pages@v3
        ```