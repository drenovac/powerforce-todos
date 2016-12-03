- Start Date: 2014-10-24
- RFC PR: https://github.com/emberjs/rfcs/pull/10
- Ember Issue: https://github.com/emberjs/ember.js/pull/12685

# Summary

Engines allow multiple logical applications to be composed together into a
single application from the user's perspective.

# Motivation

Large companies are increasingly adopting Ember.js to power their entire
product lines. Often this means separate teams (sometimes distributed
around the world) working on the same app. Typically, responsibility is
shared by dividing the application into one or more "sections". How this
division is actually implemented varies from team to team.

Sometimes, each "section" will be a completely separate Ember app, with
a shared navigation bar allowing users to switch between each app. This
allows teams to work quickly without stepping on each others' toes, but
switching apps feels slow (especially compared to the normally speedy
route transitions in Ember) because the entire page must be thrown out,
then an entirely new set of the same assets downloaded and parsed.
Additionally, code sharing is largely accomplished via copy-and-paste.

Other times, the separation is enforced socially, with each team
claiming a section of the same app in the same repository.
Unfortunately, this approach leads to frequent conflicts around shared
resources, and feedback from tests gets slower and slower as test suites
grow in size.

A more modular approach is to break off elements of a single application into
separate [addons](http://www.ember-cli.com/user-guide/#addons). Addons are
essentially mixins for [ember-cli](http://www.ember-cli.com/) applications. In
other words, the elements of an addon are merged with those of the application
that includes them. While addons allow for distributed development, testing, and
packaging, they do not provide the logical run-time separation required for
developing completely independent "sections" of an application. Addons must
function within the namespace, registry, and router of the application in which
they are included.

Engines provide an alternative to these approaches that allows for distributed
development, testing, and packaging, _as well as_ logical run-time separation.
Because engines are derived from applications, they can be just as
full-featured. Each has its own namespace and registry. Even though engines are
isolated from the applications that contain them, the boundaries between them
allow for controlled sharing of resources.

Engines can be either "routable" or "route-less":

* Routable engines provide a routing map which can be integrated with the
  routing maps of parent applications or engines. Routing maps are alway eager
  loaded, which allows for deep linking into an engine's routes regardless of
  whether the engine itself has been instantiated.

* Route-less engines can isolate complex functionality that is not related to
  routing (e.g. a chat engine in a sidebar). Route-less engines can be rendered
  into outlets ad hoc as routes are loaded.

The potential scope of engines is large enough that this feature merits
development and delivery in multiple phases. A minimum viable version could be
released sooner, which could be augmented with more advanced features later.

An initial release of engines could provide the following benefits:

* Distributed development - Engines can be developed and tested in isolation
  within their own Ember CLI projects and included by applications or other
  engines. Engines can be packaged and released as addons themselves.
