# github-cli
install github cli and login

# Example

```yml
name: Create Tag And Release
on:
  push:
    branches:
      - master
    paths-ignore:
      - '*.md'
      - 'LICENSE'
jobs:
  createTagAndRelease:
    permissions: write-all
    runs-on: ubuntu-latest
    name: Create Tag And Release
    steps:
      - name: checkout
        uses: actions/checkout@v3
        
      - name: Build
        uses: actions/setup-node@v2
        with:
          node-version: '18.x'
          registry-url: 'https://registry.npmjs.org'
      - run: npm i
      - run: npm run build
      - run: zip -r -j extension.zip ./extension/*

      - name: Set Tag Name
        uses: actions-opensource/get-next-tag@v1
               
      - name: Install GitHub CLI
        uses: actions-opensource/github-cli@v1
          
      - name: Create Tag & Release & Upload Zip Artifact To Release
        run: |
          gh release create "v${{ env.newTagName }}" --title "v${{ env.newTagName }}" --notes "Version ${{ env.newTagName }}" --latest --target "${{ github.sha }}"
          gh release upload "v${{env.newTagName}}" ./extension.zip --clobber
```
