# Copyright 2017-2019 CERN for the benefit of the ATLAS collaboration.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# Authors:
# - Cedric Serfon <cedric.serfon@cern.ch>, 2017
# - Thomas Beermann <thomas.beermann@cern.ch>, 2018
# - Mario Lassnig <mario.lassnig@cern.ch>, 2019

FROM centos:7

WORKDIR /tmp
ADD oic.rpm /tmp

RUN yum install -y epel-release.noarch
RUN yum upgrade -y
RUN yum install -y httpd mod_wsgi python-pip libaio gcc python-devel.x86_64 mod_ssl

RUN rpm -i /tmp/oic.rpm; \
    echo "/usr/lib/oracle/12.2/client64/lib" >/etc/ld.so.conf.d/oracle.conf; \
    ldconfig
RUN pip install --upgrade pip setuptools
RUN pip install rucio_webui PyMySQL cx_Oracle psycopg2-binary j2cli

ADD rucio.conf.j2 /tmp/
ADD httpd.conf /etc/httpd/conf/
ADD docker-entrypoint.sh /
RUN rm /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/userdir.conf /etc/httpd/conf.d/ssl.conf

VOLUME /var/log/httpd
VOLUME /opt/rucio/etc
VOLUME /etc/grid-security

EXPOSE 443

ENTRYPOINT ["/docker-entrypoint.sh"]
