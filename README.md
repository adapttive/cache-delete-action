# adapttive/cache-delete

## Setup

```yml
name: build-deploy

on:
  push:
    branches: [ production ]
  workflow_dispatch:

jobs:

  your-job-1:
      runs-on: ubuntu-latest
      name: 'build'

      steps:
      - name: Cache Build
        id: cache-build
        uses: actions/cache/save@v3
        with:
          path: ${{ github.workspace }}/your-folder-to-cache
          key: ${{ github.sha }}-your-cache-key

  your-job-2:
      runs-on: ubuntu-latest
      name: 'deploy'

      steps:
      - uses: actions/cache/restore@v3
        id: restore-build
        with:
          path: ${{ github.workspace }}/your-folder-to-cache
          key: ${{ github.sha }}-your-cache-key

  cleanup:
      runs-on: ubuntu-latest
      needs: ['your-job-1', 'your-job-2']
      name: 'cache cleanup'

      steps:
      - uses: adapttive/cache-delete
        with:
          github-token: ${{ secrets.ADMIN_GITHUB_TOKEN }}
          cache-key: ${{ github.sha }}-your-cache-key 
```
