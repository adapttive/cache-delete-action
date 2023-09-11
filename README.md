# cache-delete-action

## Setup

```yml
your-job-1:
    runs-on: ubuntu-latest
    name: 'production: build'

    - name: Cache Build
      id: cache-build
      uses: actions/cache/save@v3
      with:
        path: ${{ github.workspace }}/your-folder-to-cache
        key: ${{ github.sha }}-your-cache-key

cleanup:
    runs-on: ubuntu-latest
    needs: ['your-job-1']
    name: 'cache cleanup'

    steps:
    - uses: adapttive/cache-delete-action
      with:
        github-token: ${{ secrets.ADMIN_GITHUB_TOKEN }}
        cache-key: ${{ github.sha }}-your-cache-key 

```
