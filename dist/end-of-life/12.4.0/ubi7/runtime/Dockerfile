ARG IMAGE_NAME
FROM ${IMAGE_NAME}:12.4.0-base-ubi7 as base

ENV NV_CUDA_LIB_VERSION 12.4.0-1

FROM base as base-amd64

ENV NV_NVTX_VERSION 12.4.99-1
ENV NV_LIBNPP_VERSION 12.2.5.2-1
ENV NV_LIBNPP_PACKAGE libnpp-12-4-${NV_LIBNPP_VERSION}
ENV NV_LIBCUBLAS_VERSION 12.4.2.65-1
ENV NV_LIBNCCL_PACKAGE_NAME libnccl
ENV NV_LIBNCCL_PACKAGE_VERSION 2.20.5-1
ENV NV_LIBNCCL_VERSION 2.20.5
ENV NCCL_VERSION 2.20.5
ENV NV_LIBNCCL_PACKAGE ${NV_LIBNCCL_PACKAGE_NAME}-${NV_LIBNCCL_PACKAGE_VERSION}+cuda12.4
FROM base-${TARGETARCH}

ARG TARGETARCH

LABEL maintainer "NVIDIA CORPORATION <sw-cuda-installer@nvidia.com>"

RUN yum install -y \
    cuda-libraries-12-4-${NV_CUDA_LIB_VERSION} \
    cuda-nvtx-12-4-${NV_NVTX_VERSION} \
    ${NV_LIBNPP_PACKAGE} \
    libcublas-12-4-${NV_LIBCUBLAS_VERSION} \
    ${NV_LIBNCCL_PACKAGE} \
    && yum clean all \
    && rm -rf /var/cache/yum/*

# Add entrypoint items
COPY entrypoint.d/ /opt/nvidia/entrypoint.d/
COPY nvidia_entrypoint.sh /opt/nvidia/
ENV NVIDIA_PRODUCT_NAME="CUDA"
ENTRYPOINT ["/opt/nvidia/nvidia_entrypoint.sh"]