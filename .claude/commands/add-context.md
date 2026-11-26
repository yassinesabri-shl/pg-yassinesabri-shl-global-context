---
description: In the global context repository, put decision records and standards where every hub reads them, with the header the scripts read. A run takes one file, or a folder or a manifest of many. Commit them, prove the repository with check-context.mjs --global, and open one pull request for the Global Architecture Board. A standard is summarised from the standard template after every contradiction in it is settled with the person, or listed as an open item in a batch. Merges nothing and tags nothing.
argument-hint: <file> --kind adr|standard --owner <role> --revision <text> --origin <text> [--name <id>] [--status <status>] [--supersedes ADR-<NNN>] [--globs <a,b>] [--always-on] | <folder> --kind adr|standard --owner <role> --revision <text> --origin <text> [--status <status>] [--globs <a,b>] | --manifest <file>
---

<!-- Part of the harness asset library c6f3b4427d58f7c3 (see VERSION.json) · 2026-10-28 -->

# `/add-context <file> …` in the global context repository

**This file is a contract, not an integration.** It defines the procedure, the preconditions and
the stopping condition. The code host and the pull-request mechanism are yours. The command reads
a file from disk. It never fetches a page.

**Purpose.** The global context repository holds the decision records, standards and exceptions
that bind every product. Every hub reads them at a release tag. This command puts one record or
one standard here, in the form the scripts read, and opens the pull request. A batch run puts
many records and standards here in one run and one pull request (What it does — a batch). The
Global Architecture Board (the Board) reviews and merges it. A merge is a draft. A release tag
`vX.Y.Z` on the trunk makes the document bind every product. The Board tags, and this command
never does.

**Who runs it.** A member of the Board, at the root of this repository. The command dispatches no
seat. It writes one document, or one document and the status line of the record it supersedes. A
batch run writes many documents and the status line of each record they supersede.

**What it does not write.** It refuses `--kind exception`. A product team requests an exception
from its own hub, because only that hub knows its repositories and the record that states what the
product does instead. The hub's command is `/add-context --global --kind exception --departs
global.<id> --owner <role>`. It fills `hubs` from the `HUBS` line of each repository the exception
covers, takes the next number this trunk's history never held, commits, runs
`check-context.mjs --global`, and opens the pull request here on `context/EXC-<NNN>`. The Board
reviews that pull request as it reviews a record, and tags a release when the exception binds. The
command also refuses every hub kind: requirements, designs, contracts, rules and the estate
overview live in a hub or a spoke, never here.

**Ids.** A file here carries its id without a prefix: `ADR-012`, `std-caching`. Every hub reads
the same file as `global.ADR-012` and `global.std-caching`, and adds the prefix itself. Never
write `global.` into a file in this repository.

## Inputs

| Input | Form | Where it comes from |
|---|---|---|
| `<file>` | A path to one file on disk, markdown or plain text, readable in full. For `adr`, the record as the Board keeps it: the minutes or the decision page, exported to a file. For `standard`, the original standard, exported whole | The argument |
| `<folder>` | A batch. A path to a folder of markdown or plain-text files, each readable in full. The folder holds one kind, the one `--kind` names. The command reads the files in the folder only, not its subfolders | The argument, second form |
| `--manifest` | A batch. A path to a JSON file that lists every document and its values (What it does — a batch). It replaces `<file>` and `<folder>`, and it refuses `--kind`, `--name`, `--status`, `--supersedes`, `--globs` and `--always-on` on the command line | The argument, third form |
| `--kind` | `adr` or `standard`. Any other kind is refused (§ What it does not write) | The argument. The kind decides the folder |
| `--owner` | The role or the person accountable for the document being current, for example `Global Architecture Board`, or a name as the source states it | The argument |
| `--revision` | The original's version or date, as its author states it, for example `v1` or `2026-10-01`. Never the commit that adds the copy | The argument |
| `--origin` | Where the original is kept, so a reader can find it, for example `architecture board minutes, 2026-10-01` | The argument |
| `--name` | For `adr`, required: `ADR-<NNN>` (`^ADR-\d{3,}$`), the number the record carries. For `standard`: `std-<topic>` (`^std-[a-z0-9]+(?:-[a-z0-9]+)*$`). Absent, the command proposes it from the topic and shows it before the draft is written | The argument, or proposed |
| `--status` | For `adr`: `Accepted`, `Superseded by ADR-<NNN>` or `Deprecated`. For `standard`: `Active` or `Reserved` | The argument. For `standard`, `Active` when absent |
| `--supersedes` | For `adr` only: `ADR-<NNN>`, a record in `docs/architecture/` that the new record replaces | The argument |
| `--globs` | The paths in a product's repositories that the document binds, as comma-separated glob patterns, for example `**/*.go`. Empty means that a design selects the document only by its id | The argument. Empty by default |
| `--always-on` | The document binds every design of every product. Rare, and said on purpose | The argument. Off by default |

| Kind | Lands at | Written from |
|---|---|---|
| `adr` | `docs/architecture/ADR-<NNN>-<slug>.md` | `docs/architecture/_TEMPLATE.adr.md`: the header, then a summary in five sections |
| `standard` | `docs/standards/<name>.md` | `docs/standards/_TEMPLATE.standard.md`: the header, then nine sections (S1 to S8 below) |

## Preconditions

Check all seven before the command writes anything. A batch run checks preconditions 1 and 5 to 7
as written, once for the batch and once for each document, and replaces preconditions 2 to 4 with
its own (What it does — a batch).

1. The working directory is the root of the global context repository, on a clean checkout of
   its trunk. `scripts/check-context.mjs` and `.github/workflows/global-gate.yml` are there.
   `docs/estate/repositories.json` is not there, because that file makes a hub. Ask the remote
   for the trunk with `git ls-remote --symref origin HEAD`. Never assume `main`.
2. `<file>` exists, is one file, and is readable in full.
3. `--kind` is `adr` or `standard`.
4. `--owner`, `--revision` and `--origin` are given, and none is a placeholder: `TBD`, a dash,
   `unknown` or `n/a`. A document whose revision nobody knows is a document nobody can tell is
   stale.
5. The id is in its grammar, and no document in `docs/architecture/`, `docs/standards/` or
   `docs/exceptions/` carries it, except the document that this run replaces. The id is not one
   that the trunk held and then removed:

   ```bash
   git log --name-only --format= origin/<TRUNK> -- docs/architecture/ docs/standards/ docs/exceptions/
   ```

   A hub cites a document by id at a pinned commit. An id that names other bytes later is an id
   that two designs read in two ways. `check-context.mjs --global` refuses it too.
6. For `adr`: `--status` is one of the three. The record states a decision. A record that nobody
   has decided is refused: the Board decides it first. `--supersedes`, when given, names a record
   in `docs/architecture/`.
7. For `--always-on`: the person confirms it in one turn. An always-on document prints in full in
   every run of every product. The check refuses the pull request when the always-on bodies of
   the binding documents pass 8 KB, or the index lines pass 16 KB.

## What it does — a decision record

```bash
# preconditions 1–7
git fetch --quiet origin
git checkout -b context/<name> origin/<TRUNK>
# write the header and the summary (steps 1 to 3)
git add docs/architecture/
git commit -m "docs(context): add <name>" -m "Generated-by: SHL Agentic Harness"
node scripts/check-context.mjs --global          # exit 0, or fix, amend the commit, and run it again
git push -u origin context/<name>
# open the pull request, its description ending with the watermark line
```

1. **Derive the file.** `ADR-<NNN>-<slug>.md` in `docs/architecture/`, with the slug from the
   decision in a few words.
2. **Read the records already here.** Open every `Accepted` record and every `Active` standard
   whose globs overlap the new record's globs, or that is always on. When the new record
   contradicts an `Accepted` record, stop. Name both records. The Board supersedes the old record
   with `--supersedes`, or changes the new one. The command never picks between two records.
3. **Write the header, then the summary.** The header has these keys, in this order:

       ---
       name: ADR-<NNN>
       description: <one sentence, at most 200 characters: what is decided and for which code>
       globs: <a one-line JSON array, [] when --globs is absent>
       alwaysApply: <true | false>
       kind: adr
       status: <status>
       owner: <role>
       revision: <text>
       origin: <text>
       ---

   The description is what every hub's index prints, so it says what the record decides. Write
   the body from `docs/architecture/_TEMPLATE.adr.md`: Decision, Rules, Why, Rejected and Accepted
   costs, each short, and `None.` where there is nothing to say. Drop the template's comment
   block. Write only what changes how a design or code is written, from the record and nothing
   else. Never write a rule that the record does not state.
4. **Commit, then prove it.** `check-context.mjs --global` reads the commit that `HEAD` names,
   never the working tree. It exits 2 while a change under the three folders is not committed.
   So commit first, then run it. Exit 0 lets the command go on. A finding is fixed in the header or the
   summary this command wrote, never by a change to the check. Fix it, run
   `git commit --amend --no-edit`, and run the check again. The branch is not pushed yet, so the
   amendment changes no shared history.
5. **Open the pull request, and stop.** Branch `context/<name>`, commit
   `docs(context): add <name>`. The description names the kind, the status, the owner, the
   revision, the origin and the globs. It ends with this sentence and then the watermark line:

   ```text
   A merge is a draft. This record binds a product from the first release tag on a commit that carries it.

   Generated-by: SHL Agentic Harness
   ```

**Superseding a record** is this command for the new record, with `--status Accepted` and
`--supersedes ADR-<old>`. In the same commit, it changes the old record's one `status:` line to
`Superseded by ADR-<new>`, and no other byte of the old record. The check proves that the
successor is at the same commit. The old record stays, so a design that cited it at an older
release still reads it.

**Replacing a record** is this command again over the same id, with a new `--revision`. A
revision that did not change is refused by precondition 5.

## What it does — a standard

```bash
# preconditions 1–7
git log --name-only --format= origin/<TRUNK> -- docs/standards/   # every name the folder has held
# classify (S1), extract (S2), verify (S3), reconcile (S4), ask (S5), draft (S6)
git checkout -b context/<name> origin/<TRUNK>
git add docs/standards/
git commit -m "docs(context): add <name>" -m "Generated-by: SHL Agentic Harness"
node scripts/check-context.mjs --global                   # exit 0, or fix and amend (S7)
git push -u origin context/<name>
# open the pull request, its description the ingest report (S8)
```

A global standard binds every product that reads the release, so it says what the Board agreed,
once, in one shape. The original is evidence. The summary is what an agent reads.

S1. **Classify.** Read the whole file. A standard says how work is done, in statements that a
    critic can check against a design or a diff. Decide what the file is before you write:

    | The file is | Then |
    |---|---|
    | A standard | Go on to S2 |
    | A decision | Refuse, and name the home: `--kind adr` |
    | A description of a product, a design or a glossary | Refuse. It belongs in that product's hub |
    | Facts about one repository or service | Refuse. They belong in that repository |
    | A standard and something else, or two standards | Refuse, and name the parts. One standard per run |

    A refusal is the whole output: no branch, no file and no pull request.

S2. **Extract.** List every normative statement, one line each with its line number. Tag each one
    `global` (it binds every product it names, by language, path or kind of change) or `product`
    (it binds one product or one repository). Only the `global` lines go on. List the others in
    the pull request with their home. Leave narrative, history, examples and empty sections
    behind.

S3. **Verify the file against itself.** Recount every count it states. Find two statements that
    disagree. List every value it leaves open, and every claim tied to a version of a language,
    a tool or a specification. Remove personal names, e-mail addresses and handles, and keep the
    role.

S4. **Reconcile against this repository.** Open every standard whose globs overlap the new one's
    or that is always on, and every `Accepted` record whose globs overlap. Sort each rule:
    - **duplicate**: another document states it. Leave it out, and cite that document under
      Relations.
    - **refinement**: it narrows a rule of another document. Keep it, and write the relation.
    - **conflict with a record**: the record ranks above a standard. Leave the rule out, cite the
      record, and say so in the report.
    - **conflict with a standard**: the command never picks a winner. It is a question (S5).

S5. **Ask, in one message.** Ask every question whose answer changes a sentence of the summary:
    each conflict from S3 and S4, each open value, each version claim, and `Active` or `Reserved`
    when `--status` is absent. Quote both sides with their file and line, and offer the choices.
    Ask again only about what an answer opened. An answer the person does not give becomes an
    `UNRESOLVED` line under Unresolved, and the rule it would settle is not written.

S6. **Draft it from the template.** The name is `std-` and the topic in kebab case. Show the name
    before you write. The name is never one that the folder has held, and it never changes after
    the merge. Write `docs/standards/<name>.md` from `docs/standards/_TEMPLATE.standard.md`: the
    header with `kind: standard` and `status:`, then the nine sections in the template's order,
    with the comment dropped. Each kept rule is `- **<name>.<NN>** MUST …`, numbered from `.01`,
    with its source. A replacement keeps the id of a rule that did not change. The file stays
    within 200 lines. A summary that needs more is two standards: stop, and name them.

S7. **Commit, then prove it.** As step 4 of the decision record: commit, run
    `node scripts/check-context.mjs --global`, and fix, amend and run it again until it exits 0.
    The check proves the name, the file name and the title, the nine sections in order, each
    rule's id and keyword, the `UNRESOLVED` grammar and the 200-line cap.

S8. **Open the pull request, and stop.** Branch `context/<name>`, commit
    `docs(context): add <name>` (`replace <name>` for a replacement). The description is the
    ingest report, in this order:
    1. the original's title, revision and origin, and the status
    2. the counts: rules kept, left out as duplicates, and left out for a record
    3. each `product` line left behind, with its home
    4. each error found in S3, and what was done with it
    5. each question asked and its answer, word for word
    6. each `UNRESOLVED` line
    7. for a replacement, the old Must and Should rules beside the new ones
    8. the sentence `A merge is a draft. This standard binds a product from the first release tag on a commit that carries it.`
    9. the watermark line `Generated-by: SHL Agentic Harness`, alone, at the end

**Adopting a reserved standard** changes its one `status:` line to `Active`, on branch
`context/<name>-adopt`, with commit `docs(context): adopt <name>`. Run S4 again first, and ask
about each conflict it finds.

**Replacing a standard** is this command again with `--name <name>` and the new revision of the
original. S1 to S8 run in full.

## What it does — a batch

A batch loads many records and standards in one run. It gives one branch, one commit and one pull
request. Use it to load documents that already exist, for example the exports of a wiki. It
changes nothing about a single-file run. It never fetches a page: every document arrives as a
file on disk.

```bash
# batch preconditions B1–B6, on the whole set, before anything is written
git fetch --quiet origin
git log --name-only --format= origin/<TRUNK> -- docs/architecture/ docs/standards/ docs/exceptions/   # every name held
# classify and validate every document (B2, B3), reconcile the set (B4), check the size (B5)
git checkout -b <branch> origin/<TRUNK>                  # <branch> is context/bulk-<yyyymmdd>, or the first free -2, -3 (B1)
# write every document (B6)
git add docs/architecture/ docs/standards/
git commit -m "docs(context): add <n> documents" -m "Generated-by: SHL Agentic Harness"
node scripts/check-context.mjs --global                   # exit 0, or fix and amend (B7)
git push -u origin <branch>
# open one pull request (B8)
```

**The manifest** is one JSON file. `defaults` holds `owner`, `revision` and `origin`. A document
repeats a key to replace the default. `documents` lists the files.

```json
{
  "defaults": { "owner": "Global Architecture Board", "revision": "2026-10-01", "origin": "wiki export, architecture space" },
  "documents": [
    { "file": "exports/adr-012.md", "kind": "adr", "name": "ADR-012", "status": "Accepted", "globs": ["**/*.go"] },
    { "file": "exports/adr-013.md", "kind": "adr", "name": "ADR-013", "supersedes": "ADR-004" },
    { "file": "exports/caching.md", "kind": "standard", "name": "std-caching", "alwaysOn": false }
  ]
}
```

Each document takes `file` and `kind`. It may take `name`, `status`, `supersedes`, `globs`,
`alwaysOn`, `owner`, `revision` and `origin`. Any other key is a finding. A file that the manifest
does not list is never loaded. A relative `file` is read from the folder that holds the manifest,
never from the working directory, so a folder and its manifest can move together. An absolute
`file` is read as written.

**The folder form** takes `--kind`, `--owner`, `--revision` and `--origin` once for every file.
It takes `--status` and `--globs` once for every file too. A document keeps the `globs:` line of
its own header when the call gives no `--globs`, and the manifest rule is the same when a document
gives no `globs`. A file without that line has empty globs. It never takes `--name`,
`--supersedes` or `--always-on`. For `adr`, each file name starts with `ADR-<NNN>`, and that is
the record's name. For `standard`, the name is proposed from the topic (S6). A file name that does
not give an ADR number is a finding. A load that needs a supersession, an always-on document or a
different value for one file uses the manifest.

B1. **The working directory and the branch name.** Precondition 1, once. Do not check it again
    between documents. The batch writes the whole set after every check has passed, and commits
    once. Name the branch `context/bulk-<yyyymmdd>`. When that name exists on the remote
    (`git ls-remote --heads origin`) or in this clone, use the first free name that ends `-2`,
    `-3` and so on. Never reuse, reset or delete the branch of an earlier run.

B2. **Check the set before you write.** For each document, apply preconditions 2 to 7 to its own
    values. Add these checks on the set:
    - Every file exists, is readable in full, and appears once.
    - Every id is in its grammar, and no two documents carry the same id.
    - No id is already on the trunk or in the held-names list, except the document that a
      row replaces on purpose.
    - No id is in an open pull request on a branch that starts `context/`. List them with
      `gh pr list --state open --json number,headRefName,files` (**INTEGRATION POINT**: another
      host, another command), and read the ids from the paths under `docs/architecture/` and
      `docs/standards/`. Name the pull request number and the branch in the finding. Only one of
      the two pull requests can merge, so the person closes one or changes its ids. When the
      host cannot list them, write one line under Questions for the Board: the open pull
      requests were not read.
    - Every `supersedes` names a record on the trunk, or a record earlier in the same batch.
    - No document is a placeholder: `owner`, `revision` and `origin` are given, and none is `TBD`,
      a dash, `unknown` or `n/a`.
    - Each document that is `alwaysOn` is named, and the person confirms them in one turn. An
      always-on document prints in full in every run of every product.

    Print one table with a row for each document: file, kind, id, status, and every finding. Any
    finding in this step stops the batch. No branch, no file and no pull request exists after a
    stop. Fix the manifest or the folder, and run the command again.

B3. **Classify each file.** An `adr` goes through steps 1 to 3 of What it does — a decision record.
    A `standard` goes through S1 to S4 and S6. A file that fails S1 is not a finding of the batch.
    It is refused, with the home that S1 names. List it in the pull request under Refused, and
    load every other document. When no document remains, the batch stops with the same rule: no
    branch, no file and no pull request.

B4. **Reconcile the set.** Do S4 and ADR step 2 against the trunk and against the other
    documents of the batch. Two documents of the batch may contradict each other, and a new
    document may contradict an `Accepted` record. The command never picks a winner. A conflict
    between two standards, or between a standard and a record, becomes an open item (B6). A new
    record that contradicts an `Accepted` record stops the batch, as step 2 says, and names both
    records. The Board supersedes the old record first, or changes the new one.

    Sort each new record against every `Accepted` record on the trunk and every earlier record of
    the batch:
    - **Contradicts it.** The batch stops, as above.
    - **Repeats it and adds no rule.** The record is not loaded. List it under Refused, with the
      id it repeats. Every other document loads.
    - **Narrows or extends it.** The record loads, and one line under Open items names both ids.

B5. **Check the size before you commit.** Add the printed bodies of the new always-on documents
    and the index lines of the new documents to the totals the trunk already has. The caps are
    8 KB and 16 KB, and they hold for the whole repository, not for the batch. When a total
    passes a cap, stop, name the largest new documents, and ask the person which to leave out.
    Do not shorten a summary to fit.

B6. **Write every document, and defer every question.** A batch never stops to ask the S5
    questions. Each conflict, each open value and each version claim that S5 would ask about
    becomes an open item. An open value is an `UNRESOLVED` line under Unresolved, and the rule it
    would settle is not written. A proposed standard name is written as proposed. The pull request
    lists it, and the person who reviews the pull request may change it before the merge. Write the
    files as the single-file run does: the header in the same key order, the body from the
    template, and the status line of each superseded record changed in the same batch.

    Read the body of each document for the ids and paths it cites: records (`ADR-<NNN>`),
    standards and their rules (`std-<topic>`, `std-<topic>.<NN>`) and exceptions (`EXC-<NNN>`),
    and every path. A cited id is held when the trunk or the batch has it. A cited path is held
    when it is a file of this repository at the new commit. Each id or path that is not held is
    an open item that names the document and the line. A path into the harness library, such as
    `accelerator/rules/security.md`, never exists here, so the open item says so. Never remove
    a citation, and never write the missing document.
    Each standard that ends with empty globs is an open item too: a design selects it only by
    its id.

B7. **Commit once, then prove it.** Stage `docs/architecture/` and `docs/standards/`, and make
    one commit with the watermark. Then run `node scripts/check-context.mjs --global`. The check
    reads the commit that `HEAD` names. It exits 2 while a change under the three folders is not
    committed, so a half-written batch is never judged. A finding is fixed in the header or the
    summary this command wrote, never by a change to the check. Fix it, run
    `git commit --amend --no-edit`, and run the check again. The branch is not pushed yet.

B8. **Open one pull request, and stop.** The branch is the one that B1 named. The commit subject is
    `docs(context): add <n> documents`. The description has these parts, in this order:
    1. a table of every document that loaded: file, kind, id, status, owner, revision, origin
       and globs
    2. Refused: each file that S1 refused, and the home it names, and each record that B4 left
       out, with the id it repeats
    3. the S8 ingest report for each standard that loaded
    4. Open items: each conflict, each open value, each proposed standard name, each cited id
       or path that this repository does not hold, and each standard with empty globs, with the
       file and line of each side
    5. Questions for the Board: the reading of any ticket or instruction that the run could not
       settle, one line each
    6. the sentence `A merge is a draft. These documents bind a product from the first release tag on a commit that carries them.`
    7. the watermark line `Generated-by: SHL Agentic Harness`, alone, at the end

    Never tag a release, and never merge the pull request.

## After the merge

The Board tags a release on the trunk when it wants the merged documents to bind:
`vX.Y.Z`, digits only. A pre-release tag such as `v1.5.0-rc.1` binds nothing. A hub's next
`/feature` reads the newest release. An open feature moves to it only through an amendment of its
requirements.

## Never

- Never tag a release, and never merge a pull request. The Board does both.
- Never write an exception, a hub document or a spoke document here. An exception arrives from a
  hub's `/add-context --global --kind exception`.
- Never write the prefix `global.` into a file in this repository.
- Never use an id again after its document left the trunk.
- Never edit a document that this run does not place, except the one `status:` line of a record
  that the new record supersedes.
- Never run `check-context.mjs --global` before the commit. It reads `HEAD`.
- Never load a file that a manifest does not list, and never read a subfolder of a batch folder.
- Never split a batch into several branches or several pull requests, and never stop a batch to ask
  a question that can be an open item.
- Never shorten a summary to make a batch fit under a cap.
