# Asciidoctor GitHub Action

To use this action add the below config to  **.github/workflows/adocs-build.yml**

```yaml
name: build adocs

on:
  push:
    branches:
    - main
jobs:
  adoc_build:
    runs-on: ubuntu-latest
    name: asciidoctor -D docs --backend=html5 -o index.html -a toc2 docs/index.adoc 
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
    - name: Get build container
      id: adocbuild
      uses: tonynv/asciidoctor-action@master
      with:
          program: "asciidoctor -D docs --backend=html5 -o index.html docs/index.adoc"
    - name: Print execution time
      run: echo "Time ${{ steps.adocbuild.outputs.time }}"
    - name: Deploy docs to ghpages
      uses: peaceiris/actions-gh-pages@v3
      with:
        deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
        publish_branch: gh-pages
        publish_dir: ./
```
