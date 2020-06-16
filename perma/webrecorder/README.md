Sample invocation: generate the image harvardlil/webrecorder:0.05
with the command line:

```
docker build -f perma/webrecorder/Dockerfile -t harvardlil/webrecorder:0.05 .
```

To test your changes locally:
- build the image, making sure to increment the tag
- alter Perma's `docker-compose` file to point to the new version, which will exist locally on your machine. (Though the image will not yet be on Docker Hub, its local name will be identical to that future public version, so all you have to do is increment the version number.)
- spin up Perma, attempt a playback
- toggle Perma's `wr.env` to use the test-specific settings; re-run `docker-compose up`; run Perma's test suite
- rebuild the Webrecorder Docker image as necessary, and repeat
- when ready, push to Docker Hub:

```
docker push harvardlil/webrecorder:0.05
```


CHANGELOG
---------
### 0.11
- Update to Webrecorder/master@a2bc3b367c2602d2f153cbd06ea09d0eb6faec3e (latest, pre-Conifer)
- Update to latest Pywb; pin to today's digest

### 0.10
- Update to Webrecorder '4.4.0'
- Update Pywb image version to match; pin to today's digest

### 0.09
- Update to Webrecorder '4.3.0'
- Update Pywb image version to match

### 0.08
- Update to Webrecorder '4.2.1'
- Pin Pywb image version, as Webrecorder now does
- New Pywb is configured to create the archivist user using a docker entrypoint script, so that the UID/GID can be set to match the user who owns the warcs volume, which is a bind mount in standard Pywb/Webrecorder deployments. This is undesirable for Perma, since we are using named volumes, which are managed by the docker daemon and thus are normally owned by root. So, we are creating the archivist user as in earlier versions of Pywb and are overriding ENTRYPOINT to prevent the default entrypoint script from running.

### 0.07
- Update to Webrecorder 4.2.0
- Don't do any directory-related work in `Dockerfile`; will be achieved via the ENTRYPOINT script for the `recorder` container.

### 0.06
- Expose the pre-populated, chown'ed `/data` directory as a volume to
avoid permissions problems when using named volumes.


Detailed workflow for incorporating new changes from upstream (WIP)
-------------------------------------------------------------------
You should have three remotes, similar to:
```
lil git@github.com:harvard-lil/webrecorder.git (fetch)
lil git@github.com:harvard-lil/webrecorder.git (push)
origin  git@github.com:rebeccacremona/webrecorder.git (fetch)
origin  git@github.com:rebeccacremona/webrecorder.git (push)
upstream    git@github.com:webrecorder/webrecorder.git (fetch)
upstream    git@github.com:webrecorder/webrecorder.git (push)
```
1) Compare upstream and LIL's "master" branches to review changes https://github.com/harvard-lil/webrecorder/compare/master...webrecorder:master
2) Checkout `master` branch locally. Pull in upstream's changes. Push to your origin.
3) Checkout `perma` branch locally. Merge in master.
4) Bring Perma's customizations into harmony with the latest WR. May involve changes to the `perma` directory of this repo AND changes to Perma itself, especially the `services/docker/webrecorder` directory.
5) Build the harvard-lil/webrecorder Docker image; don't push it to Dockerhub yet.
6) Configure your local Perma to use the newly-built image by adjusting `docker-compose.yml` and `docker-compose-travis.yml`. Spin up some new containers.
7) Repeat 4-6 as needed.
8) Commit your changes to `perma` and push to your origin.
9) Make a PR from your origin to LIL. (URL similar to https://github.com/harvard-lil/webrecorder/compare/perma...rebeccacremona:perma?expand=1). No tests will run.
10) Make a PR to Perma. Tests will error, because the new Docker image doesn't yet exist on Github.
11) Review and merge in the WR PR.
12) Checkout the `master` branch locally, which you updated in step 2, and push to LIL remote. Should be completed by the person who opened the PR, just in case new commits were merged to master by the WR team in the meantime.
13) Push the new WR image to Dockerhub. (You'll have to build and tag first, if you aren't the person who opened the PR.)
14) Restart the Perma PR's Travis build.
15) When the tests pass, merge at will.
16) Redeploy, updating Salt config / files on the servers to match the Perma repo as necessary.
