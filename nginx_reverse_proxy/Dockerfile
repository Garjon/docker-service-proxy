FROM nginx:1.9

RUN apt-get update -qq && apt-get -y install curl zip

# Link to Consul Template Binary
ENV CT_URL https://releases.hashicorp.com/consul-template/0.12.0/consul-template_0.12.0_linux_amd64.zip

# Install Consul Template
RUN curl https://releases.hashicorp.com/consul-template/0.12.0/consul-template_0.12.0_linux_amd64.zip > consul-template-temp.zip
RUN unzip consul-template-temp.zip -d /usr/local/bin
RUN rm consul-template-temp.zip

# Setup Consul Template Files
RUN mkdir /etc/consul-templates
COPY nginx-consul-template.conf /etc/consul-templates/nginx.conf
ENV CT_FILE /etc/consul-templates/nginx.conf

# Remove all other conf files from nginx
RUN rm /etc/nginx/conf.d/*

# Setup Nginx File
ENV NX_FILE /etc/nginx/conf.d/app.conf

#Default Variables
ENV CONSUL consul:8500
ENV SERVICE consul-8500

# Command will
# 1. Write Consul Template File
# 2. Start Nginx
# 3. Start Consul Template

CMD /usr/sbin/nginx -c /etc/nginx/nginx.conf \
& CONSUL_TEMPLATE_LOG=debug consul-template \
  -consul=$CONSUL \
  -template "$CT_FILE:$NX_FILE:/usr/sbin/nginx -s reload";