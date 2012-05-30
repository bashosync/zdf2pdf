# Archiving Zendesk Based Documentation

This guide describes the process and tools for creating archived, version
specific,  Zendesk based product documentation snapshots in the form of PDF
documents which are stored in the **Archived Documentation** section of our
documentation at http://help.basho.com/.

## Process

The process for creating a documentation archive is fairly straightforward,
but involves some one-time preparation of the Zendesk account for users
making documentation archives, and some additional time to establish a working
environment for the documentation archiving script, `zdfversion.py`.

Here are the essential steps for a basic documentation archiving run:

1. Set up Zendesk API token.
2. Set up Python virtualenv to do the work in.
3. Get `zdfversion`: <TODO> add URL
4. Run `zdfversion` for the documentation set(s) you wish to archive.
5. Create a new entry in the Archived Documentation section of
Zendesk Documentation.
6. Add your generated archive PDF to the entry with any necessary notes, etc.

The details for each step above will be presented in the following sections.

## Zendesk API Token Setup

To use the `zdfversion.py` script as demonstrated in the **Utility Script**
section, you must first generate a Zendesk API token for your account. This
token helps to avoid disclosing your Zendesk user account password, and can
be regenerated or disabled altogether at any time.

To create a Zendesk API token for use with the script, follow these steps:

1. Log into your Zendesk user account at this URL: https://help.basho.com/
2. Navigate to this URL: https://help.basho.com/settings/api/
3. Click the **Enabled** checkbox inside the **Token Access** section to enable
your user account API token
4. Make note of the 40 character string after *Your API token is:*
5. Click Save

**NOTE**: If you have problems with step #3 above, you might need to ask
someone  with appropriate Zendesk fu to provide you with the
necessary permissions.

Once your Zendesk API token is configured, and you've made note of it,
proceed to configuring a Python virtual environment on the machine with which
you plan to archive documentation.

## Virtual Environment Setup

More detailed information for installation and configuring virtualenv for
specific environments is available from the
[virtualenv project page](http://pypi.python.org/pypi/virtualenv).
The most effective way to setup for using the script, is to install
virtualenv, and then follow these steps:

Create a new virtualenv:

    virtualenv zdfversion

Activate the virtualenv:

    cd zdfversion && source bin/activate

Install PyCurl:

    pip install pycurl

Obtain `zdfversion.py` from this URL: <FIXME>

Place `zdversion.py` into the virtualenv you created, and execute it per the
instructions in the Utility Script section.

## Utility Script

<FIXME> All of this needs a bit of updating

The script for creating documentation snapshots is `zdfversion.py`. The
script is simple, with the following usage synopsis:

    zdfversion.py -i <forum_id> -n "<new_forum_name>"

where the `-i` option specifies a Zendesk forum identifier, which is the 8-digit
string that makes up part of a Zendesk forum URL. For example, in the
following URL:

    https://help.basho.com/forums/20748808-operations

the forum identifier is **20748808**.

The `-n` option specifies a quoted string which will be used for the target
forum's name. The target forum will be created by the script, and entries
from the forum identifier specified by `-i` will then be copied into the
newly created target forum.

At this point, you should be able to observe that the new forum does exist in
Zendesk. You can then work with the forum like any other forum via the
web interface.

## Notes

### Details of Script Operation

<FIXME> This needs a complete refactoring to reflect new functionality

The `zdfversion.py` script performs a series of basic operations to snapshot
existing Zendesk forum entries into a newly created target forum for the
purpose of archiving documentation relevant to older product versions.

The details of its operation are as follows:

0. Creates a connection to Zendesk API
1. Fetches all entries in the forum identified by `-i` option
2. Empties the following elements from the fetched XML data so that they can
be automatically generated by the API when posted into the target forum:
3. Creates a new forum with the name specified in the `-n` option
4. Stores the forum identifier for the newly created forum
5. Posts the cleaned up XML data from source forum into new target forum


## Issues and Caveats

There are potential issues and caveats with this process:

* Storing multiple instances of the documentation in this manner could pose
problems when searching for help. For example, how do we ensure that the
results of a search are what the customer actually needs, and not a version
that is two point releases back from what they are looking for help with?

* Storing the version number of the documentation in the `<current-tags>`
element is not effective from a reader's perspective in our present
implementation of Zendesk forums, as tags are not visibly rendered to the
page, and so the information they convey is hidden to the reader.

* The `zdfversion.py` is a very simple implementation, and uses only the
minimal Zendesk API calls to function, so for example, it cannot be used to
delete an "oops" when creating a new forum and snapshotting documentation
into it.

* The `zdfversion.py` script does not currently handle additional forum
metadata such as comments or file attachments, but it is a planned capability
for a subsequent version.

* There are some bits left in the script for other operations which are not
part of the current process. These will be cleaned up soon.
