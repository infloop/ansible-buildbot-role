# -*- python -*-
# ex: set filetype=python:

import os
import re
import json

import slack

from buildbot.plugins import *
from buildbot.plugins import webhooks

cached_twlog = None
def getTwlog():
  global cached_twlog
  if cached_twlog:
    return cached_twlog
  else:
    from twisted.python import log as twlog
    cached_twlog = twlog
    return cached_twlog

# This is a sample buildmaster config file. It must be installed as
# 'master.cfg' in your buildmaster's base directory.

# This is the dictionary that the buildmaster pays attention to. We also use
# a shorter alias to save typing.
c = BuildmasterConfig = {}

####### WORKERS

# The 'workers' list defines the set of recognized workers. Each element is
# a Worker object, specifying a unique worker name and password.  The same
# worker name and password must be configured on the worker.

c['workers'] = []
{% for worker in buildbot_workers %}
c['workers'].append(worker.Worker('{{ worker.name }}', '{{ worker.password }}'))
{% endfor %}

if 'BUILDBOT_MQ_URL' in os.environ:
    c['mq'] = {
        'type' : 'wamp',
        'router_url': os.environ['BUILDBOT_MQ_URL'],
        'realm': os.environ.get('BUILDBOT_MQ_REALM', 'buildbot').decode('utf-8'),
        'debug' : 'BUILDBOT_MQ_DEBUG' in os.environ,
        'debug_websockets' : 'BUILDBOT_MQ_DEBUG' in os.environ,
        'debug_lowlevel' : 'BUILDBOT_MQ_DEBUG' in os.environ,
    }

# 'protocols' contains information about protocols which master will use for
# communicating with workers. You must define at least 'port' option that workers
# could connect to your master with this protocol.
# 'port' must match the value configured into the workers (with their
# --master option)
c['protocols'] = {'pb': {'port': os.environ.get("BUILDBOT_WORKER_PORT", 9989)}}

####### SECRETS PROVIDERS

c['secretsProviders'] = []
{% if (buildbot_master_secrets_vault is defined) and (buildbot_master_secrets_vault == "present") %}
c['secretsProviders'].append(
  secrets.HashiCorpVaultSecretProvider(
    vaultToken = "{{ buildbot_master_secrets_vault_token }}",
    vaultServer = "{{ buildbot_master_secrets_vault_url }}",
    secretsmount = "{{ buildbot_master_secrets_vault_mount }}",
    apiVersion = {{ buildbot_master_secrets_vault_api_version }}
  )
)
{% endif %}

####### CHANGESOURCES

# the 'change_source' setting tells the buildmaster how it should find out
# about source code changes.  Here we point to the buildbot clone of pyflakes.

c['change_source'] = []

####### SCHEDULERS

# Configure the Schedulers, which decide how to react to incoming changes.  In this
# case, just kick off a 'runtests' build

c['schedulers'] = []

####### BUILDERS

# The 'builders' list defines the Builders, which tell Buildbot how to perform a build:
# what steps, and which workers can execute them.  Note that any particular build will
# only take place on one worker.

factory = util.BuildFactory()

c['builders'] = []

####### SERVICES

c['services'] = []
{% if (buildbot_master_slack is defined) and (buildbot_master_slack == "present") %}
c['services'].append(
  slack.SlackStatusPush(
    endpoint = "{{ buildbot_master_slack_url }}",
    channel = "{{ buildbot_master_slack_channel }}",
  )
)
{% endif %}

####### STATUS TARGETS

# 'status' is a list of Status Targets. The results of each build will be
# pushed to these targets. buildbot/status/*.py has a variety to choose from,
# like IRC bots.

c['status'] = []

####### PROJECT IDENTITY

# the 'title' string will appear at the top of this buildbot installation's
# home pages (linked to the 'titleURL').

c['title'] = "{{ buildbot_web_ui_title }}"
c['titleURL'] = "{{ buildbot_web_url }}"

# the 'buildbotURL' string should point to the location where the buildbot's
# internal web server is visible. This typically uses the port number set in
# the 'www' entry below, but with an externally-visible host name which the
# buildbot cannot figure out without some help.

c['buildbotURL'] = os.environ.get("BUILDBOT_WEB_URL", "http://localhost:8010/")

# minimalistic config to activate new web UI
c['www'] = dict(
  port = os.environ.get("BUILDBOT_WEB_PORT", 8010),
  plugins = dict(waterfall_view = {}, console_view = {}),
  change_hook_dialects = {
    'base' : True
  },
)

####### Auth

c['www']['authz'] = util.Authz(
  allowRules = [
    util.AnyEndpointMatcher(role="admins", defaultDeny=False),

    util.StopBuildEndpointMatcher(role="users"),
    util.RebuildBuildEndpointMatcher(role="users"),
    util.ForceBuildEndpointMatcher(role="users"),
    util.StopBuildEndpointMatcher(role="users"),

    util.EnableSchedulerEndpointMatcher(role="admins"),
    util.AnyControlEndpointMatcher(role="admins")
  ],
  roleMatchers = [
    util.RolesFromUsername(roles=['admins'], usernames=[
      {% for user in (buildbot_master_users | selectattr("role", "equalto", "admins") | list) %}
      '{{ user.login }}',
      {% endfor %}
    ]),
    util.RolesFromUsername(roles=['users'], usernames=[
      {% for user in (buildbot_master_users | selectattr("role", "equalto", "users") | list) %}
      '{{ user.login }}',
      {% endfor %}
    ]),
    # role owner is granted when property owner matches the email of the user
    util.RolesFromOwner(role="owner")
  ]
)
c['www']['auth'] = util.UserPasswordAuth({
  {% for user in buildbot_master_users %}
  '{{ user.login }}': '{{ user.password }}',
  {% endfor %}
})

####### DB URL

c['db'] = {
    # This specifies what database buildbot uses to store its state.  You can leave
    # this at its default for all but the largest installations.
    'db_url' : os.environ.get("BUILDBOT_DB_URL", "sqlite://").format(**os.environ),
}
