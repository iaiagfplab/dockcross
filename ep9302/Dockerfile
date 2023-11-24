ARG ORG=dockcross
FROM ${ORG}/base:latest

LABEL maintainer="Matt McCormick matt.mccormick@kitware.com"

# Crosstool-ng version 2022-05-19
ENV CT_VERSION crosstool-ng-1.25.0

# This is for Cirrus Logic ep9302
#include "common.crosstool"
# The cross-compiling emulator
RUN apt-get update \
&& apt-get install -y \
  qemu-user \
  qemu-user-static \
  install lib32z1 \
&& apt-get clean --yes


# The CROSS_TRIPLE is a configured alias of the "arm-linux-uclibc" target.
ENV CROSS_TRIPLE arm-linux-uclibc
ENV CROSS_ROOT ${XCC_PREFIX}/${CROSS_TRIPLE}
COPY ./4.1.1-920t/bin/* ${CROSS_ROOT}/bin/
ENV AS=${CROSS_ROOT}/bin/${CROSS_TRIPLE}-as \
    AR=${CROSS_ROOT}/bin/${CROSS_TRIPLE}-ar \
    CC=${CROSS_ROOT}/bin/${CROSS_TRIPLE}-gcc \
    CPP=${CROSS_ROOT}/bin/${CROSS_TRIPLE}-cpp \
    CXX=${CROSS_ROOT}/bin/${CROSS_TRIPLE}-g++ \
    LD=${CROSS_ROOT}/bin/${CROSS_TRIPLE}-ld \
    FC=${CROSS_ROOT}/bin/${CROSS_TRIPLE}-gfortran

ENV QEMU_LD_PREFIX "${CROSS_ROOT}/${CROSS_TRIPLE}/sysroot"
ENV QEMU_SET_ENV "LD_LIBRARY_PATH=${CROSS_ROOT}/lib:${QEMU_LD_PREFIX}"

COPY Toolchain.cmake ${CROSS_ROOT}/
ENV CMAKE_TOOLCHAIN_FILE ${CROSS_ROOT}/Toolchain.cmake

ENV PKG_CONFIG_PATH /usr/lib/arm-linux-uclibc/

# Linux kernel cross compilation variables
ENV PATH ${PATH}:${CROSS_ROOT}/bin
ENV CROSS_COMPILE ${CROSS_TRIPLE}-
ENV ARCH arm

#include "common.label-and-env"
