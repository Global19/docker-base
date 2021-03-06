# Docker image for nextstrain/base

This is the source for creating the `nextstrain/base` Docker image.  Currently
the image is published as [`nextstrain/base`][].

Ideally most pathogen builds are supported by this base image without further
customization.  The possibility remains, however, for pathogens to define and
use an image derived from this base layer.  This would be desirable for
pathogen builds requiring custom external software, like Python modules or
tree-builders.

The image includes the standard Nextstrain components Fauna, Augur, and Auspice,
as well as other bioinformatics tools like MAFFT, RAxML, FastTree, IQ-TREE, and
TreeTime.

This image is best interacted with using the [Nextstrain command-line
tool][nextstrain-cli].

[nextstrain-cli]: https://github.com/nextstrain/cli


## Developing

### Building

To build this image locally, first pull down the latest image from Docker Hub:

    ./devel/pull

This will save you time by taking advantage of layer caching.  Then build the
image with:

    ./devel/build

On each subsequent change during your development iterations, you can run just
the `./devel/build` command again.

If you need to force the cached Nextstrain layers to rebuild to, for example,
pick up a new version of augur or auspice, set the `CACHE_DATE` environment
variable to a new timestamp first:

    export CACHE_DATE=$(date --utc +%Y%m%dT%H%M%SZ)

Otherwise, letting the build process use the cached layers will save you time
during development iterations.

### Pushing images to Docker Hub

To push images you've built locally to Docker Hub, you can run:

    ./devel/push latest

This will publish your local `nextstrain/base:latest` image.  This is also what
happens if you run `./devel/push` with no tags specified.  If you have images
with other tags, you may provide those tags in addition to or instead of
`latest`.

### Best practices

The smaller the image size, the better.  To this end we build upon a ["slim"
Python image][] and use a [multi-stage build][] where only _artifacts_ are
included in the final image without any of the software required only for
compiling, installing, building, etc.

Try to follow [Docker best practices][] for images, although not all apply to our
use case, which is somewhat atypical.

The [Dockerfile reference documentation][] is quite handy for looking up the
details of each Dockerfile command (`COPY`, `ADD`, etc).

### Continuous integration

Every push to this repository triggers a new build of the image on [Travis
CI][].  This helps ensure the image builds successfully with the new commits.

Images built from the `master` branch are additionally pushed to the [Docker
registry][`nextstrain/base`].  The build instructions used by Travis are in
this repo's `.travis.yml`.


[`nextstrain/base`]: https://hub.docker.com/r/nextstrain/base/
["slim" Python image]: https://hub.docker.com/_/python
[multi-stage build]: https://docs.docker.com/develop/develop-images/multistage-build/
[Docker best practices]: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
[Dockerfile reference documentation]: https://docs.docker.com/engine/reference/builder/
[Travis CI]: https://travis-ci.com/nextstrain/docker-base/
