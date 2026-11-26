---
name: EXC-<NNN>
description: <one sentence, at most 200 characters: which product departs from which record, and why>
kind: exception
hubs: <a one-line JSON array of hub key prefixes, for example ["SIM"]>
repositories: <a one-line JSON array of repository ids, for example ["sim-desktop"]>
departs: <a one-line JSON array of the records and standard rules it departs from, for example ["ADR-012", "std-go.03"]>
status: Active
review-by: <YYYY-MM-DD, the date the Board reads the exception again>
owner: <the role that requested it, never a person>
approver: Global Architecture Board
revision: v1
origin: this repository
---

<!--
  Exception template, v1.0 — the global context repository's own document.

  An exception records that the hubs in `hubs` may depart from the global
  records and standard rules in `departs`, in the repositories in
  `repositories`, within the written scope, until the review date. Only the
  Board merges it, and a release tag makes it binding. A product requests it
  from its hub. It never grants one to itself.

  The header is read by scripts, so each value is exact:
   - hubs: one key prefix or more, the prefix in each hub's
     docs/backlog/keys.json. An exception that covers a repository more than
     one hub owns names every one of those hubs.
   - repositories: one repository id or more, as the hubs list them in
     docs/estate/repositories.json.
   - departs: one id or more, as this repository carries them: a record
     ADR-<NNN>, or one rule of a standard std-<topic>.<NN>. Each must be in this
     repository at the same commit, and the rule must be one the standard
     defines.
   - status: Active or Withdrawn. Only an Active exception has effect. A
     withdrawn one stays, so a design that cited it still reads it.
   - review-by: a date. Past it, a hub's gate reports the exception as stale
     and still passes.

  Reason, Scope and Alternatives considered are required. The check
  (scripts/check-context.mjs --global, in global-gate) refuses an empty
  section, a placeholder or an UNRESOLVED line in any of the three. The other
  two sections are for the Board's reader.

  Never edit an exception to widen its scope. A wider departure is a new
  exception with a new number, approved again. Never use a number again, even
  after its file is removed.

  Delete this comment once the exception is written.
-->

# EXC-<NNN> — <the departure, in a few words>

## Reason

<Why the product cannot follow the record. State the fact that prevents it, with its source.>

## Scope

<Which kind of change the exception covers, in the listed repositories. Outside this scope, the
record binds as before. Name what the exception does not cover.>

## Alternatives considered

- <An option the team tried or rejected.> Rejected: <the reason, with a number where one exists>.

## What the product does instead

<The hub record that states the product's own decision, by its id, for example ADR-031.>

## Exit

<The condition that ends the exception, for example "the simulator gains a network mode".>
