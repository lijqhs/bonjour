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
