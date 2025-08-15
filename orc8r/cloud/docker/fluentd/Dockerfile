# Copyright 2023 The Magma Authors.

# This source code is licensed under the BSD-style license found in the
# LICENSE file in the root directory of this source tree.

# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# Copyright 2023 The Magma Authors.
# Licensed under the BSD-style license in the LICENSE file.


# Use an official Ruby 3.2.2 image as the base to ensure compatibility
# with Fluentd plugins that require Ruby >= 3.0.
FROM ruby:3.2.2-bullseye

# Install system dependencies required to build Fluentd and its native extensions.
# - build-essential: compilation tools
# - curl: download files
# - git: source control
# - ca-certificates: SSL support
# - libssl-dev & libffi-dev: required by some gems
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        build-essential \
        curl \
        git \
        ca-certificates \
        libssl-dev \
        libffi-dev && \
    # Install Fluentd version 1.14.6 via gem
    gem install fluentd -v 1.14.6 --no-document && \
    # Clean up APT cache to reduce image size
    rm -rf /var/lib/apt/lists/*

# Install required Fluentd plugins:
# - multi_json pinned to 1.15.0 to maintain compatibility with Ruby 3.x
# - elasticsearch and multi-format-parser plugins
RUN gem install multi_json -v 1.15.0 --no-document && \
    gem install \
        elasticsearch:7.13.0 \
        fluent-plugin-elasticsearch:5.2.1 \
        fluent-plugin-multi-format-parser:1.0.0 \
        --no-document

# Create a non-root user 'fluent' for running Fluentd securely
RUN useradd -ms /bin/bash fluent
USER fluent

# Set the working directory inside the container
WORKDIR /fluentd
