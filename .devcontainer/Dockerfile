FROM ros:jazzy

RUN mkdir -p /ros2_ws/src/pamiq_ros
WORKDIR /ros2_ws/src/pamiq_ros
COPY ./ ./

# Setup dependencies
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    git \
    curl \
    make \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/*

# Setup ros2
RUN echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc \
&& echo 'if [ -f /ros2_ws/install/setup.bash ]; then \
source /ros2_ws/install/setup.bash; \
fi' >> ~/.bashrc

# Setup uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/
ENV UV_LINK_MODE=copy
RUN echo 'eval "$(uv generate-shell-completion bash)"' >> ~/.bashrc \
&& make venv \
&& uv run pre-commit install

# Console setup
CMD [ "bash" ]
