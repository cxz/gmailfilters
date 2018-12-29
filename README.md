# gmailfilters

[![Travis CI](https://img.shields.io/travis/jessfraz/gmailfilters.svg?style=for-the-badge)](https://travis-ci.org/jessfraz/gmailfilters)
[![GoDoc](https://img.shields.io/badge/godoc-reference-5272B4.svg?style=for-the-badge)](https://godoc.org/github.com/jessfraz/gmailfilters)
[![Github All Releases](https://img.shields.io/github/downloads/jessfraz/gmailfilters/total.svg?style=for-the-badge)](https://github.com/jessfraz/gmailfilters/releases)

A tool to sync Gmail filters from a config file to your account.

* [Installation](README.md#installation)
   * [Binaries](README.md#binaries)
   * [Via Go](README.md#via-go)
* [Usage](README.md#usage)
* [Example Filter File](README.md#example-filter-file)
* [Setup](README.md#setup)
   * [Gmail](README.md#gmail)

## Installation

#### Binaries

For installation instructions from binaries please visit the [Releases Page](https://github.com/jessfraz/gmailfilters/releases).

#### Via Go

```console
$ go get github.com/jessfraz/gmailfilters
```

## Usage

```console
$ gmailfilters -h
gmailfilters -  A tool to sync Gmail filters from a config file to your account.

Usage: gmailfilters <command>

Flags:

  -d, --debug       enable debug logging (default: false)
  -f, --creds-file  Gmail credential file (or env var GMAIL_CREDENTIAL_FILE) (default: <none>)

Commands:

  version  Show the version information.
```

## Example Filter File

```toml
[[filter]]
query = "to:your_activity@noreply.github.com"
archive = true
read = true

[[filter]]
query = "from:notifications@github.com LGTM"
label = "github/LGTM"

[[filter]]
query = """
(-to:team_mention@noreply.github.com \
(from:(notifications@github.com) AND (@jfrazelle OR @jessfraz OR to:mention@noreply.github.com OR to:author@noreply.github.com OR to:assign@noreply.github.com)))
"""
label = "github/mentions"

[[filter]]
query = """
to:team_mention@noreply.github.com \
-to:mention@noreply.github.com \
-to:author@noreply.github.com \
-to:assign@noreply.github.com
"""
label = "github/team-mention"

[[filter]]
query = """
from:notifications@github.com \
-to:team_mention@noreply.github.com \
-to:mention@noreply.github.com \
-to:author@noreply.github.com \
-to:assign@noreply.github.com
"""
archive = true

[[filter]]
query = "(from:me AND to:reply@reply.github.com)"
label = "github/mentions"

[[filter]]
query = "(from:notifications@github.com)"
label = "github"

[[filter]]
queryOr = [
"to:plans@tripit.com",
"to:receipts@concur.com",
"to:plans@concur.com",
"to:receipts@expensify.com"
]
delete = true

[[filter]]
queryOr = [
"from:notifications@docker.com",
"from:noreply@github.com",
"from:builds@travis-ci.org"
]
label = "to-be-deleted"

[[filter]]
query = "drive-shares-noreply@google.com OR (subject:\"Invitation to comment\" AND from:me ) OR from:(*@docs.google.com)"
label = "to-be-deleted"

[[filter]]
query = "(from:(-me) {filename:vcs filename:ics} has:attachment) OR (subject:(\"invitation\" OR \"accepted\" OR \"tentatively accepted\" OR \"rejected\" OR \"updated\" OR \"canceled event\" OR \"declined\") when where calendar who organizer)"
label = "to-be-deleted"
```

## Setup

### Gmail

1. Enable the API: To get started using Gmail API, you need to 
    first create a project in the 
    [Google API Console](https://console.developers.google.com),
    enable the API, and create credentials.

    Follow the instructions 
    [for step enabling the API here](https://developers.google.com/gmail/api/quickstart/go).
