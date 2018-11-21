# ADD instruction Dockerfile for Docker Quick Start
FROM alpine:latest
LABEL maintainer="Earl Waud <earlwaud@mycompany.com>"
LABEL version=1.0
ADD file* theqsg/files/
ADD folder1 theqsg/
WORKDIR theqsg
ADD --chown=35:35 special1 special-files/
WORKDIR /
CMD ["sh"]
