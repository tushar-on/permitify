FROM andrewwhitehead/von-image:py36-indy1.3.1-dev-441-ew-s2i

# The indy (1001) user has already been setup for us at this point.

USER root

RUN mkdir -p $HOME/.indy_client/wallet

# =======================================================================
# Our OpenShift 3.7/3.9 environments don't support the --chown flag.
# ---
# Docker Engine goes up to 1.13.1 (last release), 
# all newer releases are called  Docker CE or Docker EE starting at 
# version 17.03.0.
#
# Openshift 3.7 => docker engine 1.12
# Openshift 3.9 => docker engine 1.13
# -----------------------------------------------------------------------
#
# COPY --chown=indy:indy ./src/requirements.txt $HOME/
# RUN pip install -r requirements.txt
#
# ADD --chown=indy:indy ./src $HOME
# ADD --chown=indy:indy ./site_templates $HOME/site_templates
# ADD --chown=indy:indy ./docker/docker-entrypoint.sh /usr/local/bin/enter
# 
# RUN chown -R indy:indy $HOME/.indy_client && \
#     chmod -R a+rwx /usr/local/bin && \
#     chmod -R a+rw $HOME/.indy_client && \
#     chmod a+rw $HOME#
# -----------------------------------------------------------------------
# The workaround ...
# -----------------------------------------------------------------------
COPY ./src/requirements.txt $HOME/
RUN pip install -r requirements.txt

ADD ./src $HOME
ADD ./site_templates $HOME/site_templates
ADD ./docker/docker-entrypoint.sh /usr/local/bin/enter

# Selectively chown the files we just added.
# Doing a recursive chown on $HOME would be redundant and time consuming.
RUN chown indy:indy $HOME/* /usr/local/bin/enter && \
    chown -R indy:indy $HOME/.indy_client $HOME/site_templates $HOME/permitify $HOME/static $HOME/templates $HOME/von_connector && \
    chmod -R a+rwx /usr/local/bin && \
    chmod -R a+rw $HOME/.indy_client && \
    chmod a+rw $HOME
# =======================================================================

USER 1001

ENTRYPOINT ["enter"]