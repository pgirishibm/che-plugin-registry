# Copyright (c) 2019 Red Hat, Inc.
# This program and the accompanying materials are made
# available under the terms of the Eclipse Public License 2.0
# which is available at https://www.eclipse.org/legal/epl-2.0/
#
# SPDX-License-Identifier: EPL-2.0
#
# Contributors:
#   Red Hat, Inc. - initial API and implementation

FROM python:3.8.6-slim

ENV HOME=/home/theia

RUN mkdir /projects ${HOME} && \
    # Change permissions to let any arbitrary user
    for f in "${HOME}" "/etc/passwd" "/projects"; do \
      echo "Changing permissions on ${f}" && chgrp -R 0 ${f} && \
      chmod -R g+rwX ${f}; \
    done

RUN apt-get update && \
    apt-get install exuberant-ctags && \
    apt-get install wget -y && \
    wget -O - https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get update && \
    apt-get install nodejs gcc build-essential -y && \
    pip install jupyter && \
    pip install pylint python-language-server[all] ptvsd jedi && \
    apt-get purge -y --auto-remove gcc build-essential && \
    apt-get clean && apt-get -y autoremove && rm -rf /var/lib/apt/lists/*

ADD etc/entrypoint.sh /entrypoint.sh
ENTRYPOINT [ "/entrypoint.sh" ]
CMD ${PLUGIN_REMOTE_ENDPOINT_EXECUTABLE}
