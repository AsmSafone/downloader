# CONTRIBUTING TO YT-DLP

- [OPENING AN ISSUE](#opening-an-issue)
    - [Is the description of the issue itself sufficient?](#is-the-description-of-the-issue-itself-sufficient)
    - [Are you using the latest version?](#are-you-using-the-latest-version)
    - [Is the issue already documented?](#is-the-issue-already-documented)
    - [Why are existing options not enough?](#why-are-existing-options-not-enough)
    - [Have you read and understood the changes, between youtube-dl and yt-dlp](#have-you-read-and-understood-the-changes-between-youtube-dl-and-yt-dlp)
    - [Is there enough context in your bug report?](#is-there-enough-context-in-your-bug-report)
    - [Does the issue involve one problem, and one problem only?](#does-the-issue-involve-one-problem-and-one-problem-only)
    - [Is anyone going to need the feature?](#is-anyone-going-to-need-the-feature)
    - [Is your question about yt-dlp?](#is-your-question-about-yt-dlp)
- [DEVELOPER INSTRUCTIONS](#developer-instructions)
    - [Adding new feature or making overarching changes](#adding-new-feature-or-making-overarching-changes)
    - [Adding support for a new site](#adding-support-for-a-new-site)
    - [yt-dlp coding conventions](#yt-dlp-coding-conventions)
        - [Mandatory and optional metafields](#mandatory-and-optional-metafields)
        - [Provide fallbacks](#provide-fallbacks)
        - [Regular expressions](#regular-expressions)
        - [Long lines policy](#long-lines-policy)
        - [Inline values](#inline-values)
        - [Collapse fallbacks](#collapse-fallbacks)
        - [Trailing parentheses](#trailing-parentheses)
        - [Use convenience conversion and parsing functions](#use-convenience-conversion-and-parsing-functions)
- [EMBEDDING YT-DLP](README.md#embedding-yt-dlp)



# OPENING AN ISSUE

Bugs and suggestions should be reported at: [yt-dlp/yt-dlp/issues](https://github.com/yt-dlp/yt-dlp/issues). Unless you were prompted to or there is another pertinent reason (e.g. GitHub fails to accept the bug report), please do not send bug reports via personal email. For discussions, join us in our [discord server](https://discord.gg/H5MNcFW63r).

**Please include the full output of yt-dlp when run with `-Uv`**, i.e. **add** `-Uv` flag to **your command line**, copy the **whole** output and post it in the issue body wrapped in \`\`\` for better formatting. It should look similar to this:
```
$ yt-dlp -Uv <your command line>
[debug] Command-line config: ['-v', 'demo.com']
[debug] Encodings: locale UTF-8, fs utf-8, out utf-8, pref UTF-8
[debug] yt-dlp version 2021.09.25 (zip)
[debug] Python version 3.8.10 (CPython 64bit) - Linux-5.4.0-74-generic-x86_64-with-glibc2.29
[debug] exe versions: ffmpeg 4.2.4, ffprobe 4.2.4
[debug] Proxy map: {}
Current Build Hash 25cc412d1d3c0725a1f2f5b7e4682f6fb40e6d15f7024e96f7afd572e9919535
yt-dlp is up to date (2021.09.25)
...
```
**Do not post screenshots of verbose logs; only plain text is acceptable.**

The output (including the first lines) contains important debugging information. Issues without the full output are often not reproducible and therefore will be closed as `incomplete`.

The templates provided for the Issues, should be completed and **not removed**, this helps aide the resolution of the issue.

Please re-read your issue once again to avoid a couple of common mistakes (you can and should use this as a checklist):

### Is the description of the issue itself sufficient?

We often get issue reports that we cannot really decipher. While in most cases we eventually get the required information after asking back multiple times, this poses an unnecessary drain on our resources.

So please elaborate on what feature you are requesting, or what bug you want to be fixed. Make sure that it's obvious

- What the problem is
- How it could be fixed
- How your proposed solution would look like

If your report is shorter than two lines, it is almost certainly missing some of these, which makes it hard for us to respond to it. We're often too polite to close the issue outright, but the missing info makes misinterpretation likely. We often get frustrated by these issues, since the only possible way for us to move forward on them is to ask for clarification over and over.

For bug reports, this means that your report should contain the **complete** output of yt-dlp when called with the `-Uv` flag. The error message you get for (most) bugs even says so, but you would not believe how many of our bug reports do not contain this information.

If the error is `ERROR: Unable to extract ...` and you cannot reproduce it from multiple countries, add `--write-pages` and upload the `.dump` files you get [somewhere](https://gist.github.com).

**Site support requests must contain an example URL**. An example URL is a URL you might want to download, like `https://www.youtube.com/watch?v=BaW_jenozKc`. There should be an obvious video present. Except under very special circumstances, the main page of a video service (e.g. `https://www.youtube.com/`) is *not* an example URL.

###  Are you using the latest version?

Before reporting any issue, type `yt-dlp -U`. This should report that you're up-to-date. This goes for feature requests as well.

###  Is the issue already documented?

Make sure that someone has not already opened the issue you're trying to open. Search at the top of the window or browse the [GitHub Issues](https://github.com/yt-dlp/yt-dlp/search?type=Issues) of this repository. If there is an issue, feel free to write something along the lines of "This affects me as well, with version 2021.01.01. Here is some more information on the issue: ...". While some issues may be old, a new post into them often spurs rapid activity.

Additionally, it is also helpful to see if the issue has already been documented in the [youtube-dl issue tracker](https://github.com/ytdl-org/youtube-dl/issues). If similar issues have already been reported in youtube-dl (but not in our issue tracker), links to them can be included in your issue report here.

###  Why are existing options not enough?

Before requesting a new feature, please have a quick peek at [the list of supported options](README.md#usage-and-options). Many feature requests are for features that actually exist already! Please, absolutely do show off your work in the issue report and detail how the existing similar options do *not* solve your problem.

###  Have you read and understood the changes, between youtube-dl and yt-dlp

There are many changes between youtube-dl and yt-dlp [(changes to default behavior)](README.md#differences-in-default-behavior), and some of the options available have a different behaviour in yt-dlp, or have been removed all together [(list of changes to options)](README.md#deprecated-options). Make sure you have read and understand the differences in the options and how this may impact your downloads before opening an issue.

###  Is there enough context in your bug report?

People want to solve problems, and often think they do us a favor by breaking down their larger problems (e.g. wanting to skip already downloaded files) to a specific request (e.g. requesting us to look whether the file exists before downloading the info page). However, what often happens is that they break down the problem into two steps: One simple, and one impossible (or extremely complicated one).

We are then presented with a very complicated request when the original problem could be solved far easier, e.g. by recording the downloaded video IDs in a separate file. To avoid this, you must include the greater context where it is non-obvious. In particular, every feature request that does not consist of adding support for a new site should contain a use case scenario that explains in what situation the missing feature would be useful.

###  Does the issue involve one problem, and one problem only?

Some of our users seem to think there is a limit of issues they can or should open. There is no limit of issues they can or should open. While it may seem appealing to be able to dump all your issues into one ticket, that means that someone who solves one of your issues cannot mark the issue as closed. Typically, reporting a bunch of issues leads to the ticket lingering since nobody wants to attack that behemoth, until someone mercifully splits the issue into multiple ones.

In particular, every site support request issue should only pertain to services at one site (generally under a common domain, but always using the same backend technology). Do not request support for vimeo user videos, White house podcasts, and Google Plus pages in the same issue. Also, make sure that you don't post bug reports alongside feature requests. As a rule of thumb, a feature request does not include outputs of yt-dlp that are not immediately related to the feature at hand. Do not post reports of a network error alongside the request for a new video service.

###  Is anyone going to need the feature?

Only post features that you (or an incapacitated friend you can personally talk to) require. Do not post features because they seem like a good idea. If they are really useful, they will be requested by someone who requires them.

###  Is your question about yt-dlp?

Some bug reports are completely unrelated to yt-dlp and relate to a different, or even the reporter's own, application. Please make sure that you are actually using yt-dlp. If you are using a UI for yt-dlp, report the bug to the maintainer of the actual application providing the UI. In general, if you are unable to provide the verbose log, you should not be opening the issue here.

If the issue is with `youtube-dl` (the upstream fork of yt-dlp) and not with yt-dlp, the issue should be raised in the youtube-dl project.

### Are you willing to share account details if needed?

The maintainers and potential contributors of the project often do not have an account for the website you are asking support for. So any developer interested in solving your issue may ask you for account details. It is your personal discression whether you are willing to share the account in order for the developer to try and solve your issue. However, if you are unwilling or unable to provide details, they obviously cannot work on the issue and it cannot be solved unless some developer who both has an account and is willing/able to contribute decides to solve it.

By sharing an account with anyone, you agree to bear all risks associated with it. The maintainers and yt-dlp can't be held responsible for any misuse of the credentials.

While these steps won't necessarily ensure that no misuse of the account takes place, these are still some good practices to follow.

- Look for people with `Member` (maintainers of the project) or `Contributor` (people who have previously contributed code) tag on their messages.
- Change the password before sharing the account to something random (use [this](https://passwordsgenerator.net/) if you don't have a random password generator).
- Change the password after receiving the account back.




# DEVELOPER INSTRUCTIONS

Most users do not need to build yt-dlp and can [download the builds](https://github.com/yt-dlp/yt-dlp/releases) or get them via [the other installation methods](README.md#installation).

To run yt-dlp as a developer, you don't need to build anything either. Simply execute

    python -m yt_dlp

To run the test, simply invoke your favorite test runner, or execute a test file directly; any of the following work:

    python -m unittest discover
    python test/test_download.py
    nosetests
    pytest

See item 6 of [new extractor tutorial](#adding-support-for-a-new-site) for how to run extractor specific test cases.

If you want to create a build of yt-dlp yourself, you can follow the instructions [here](README.md#compile).


## Adding new feature or making overarching changes

Before you start writing code for implementing a new feature, open an issue explaining your feature request and atleast one use case. This allows the maintainers to decide whether such a feature is desired for the project in the first place, and will provide an avenue to discuss some implementation details. If you open a pull request for a new feature without discussing with us first, do not be surprised when we ask for large changes to the code, or even reject it outright.

The same applies for changes to the documentation, code style, or overarching changes to the architecture


## Adding support for a new site

If you want to add support for a new site, first of all **make sure** this site is **not dedicated to [copyright infringement](https://www.github.com/ytdl-org/youtube-dl#can-you-add-support-for-this-anime-video-site-or-site-which-shows-current-movies-for-free)**. yt-dlp does **not support** such sites thus pull requests adding support for them **will be rejected**.

After you have ensured this site is distributing its content legally, you can follow this quick list (assuming your service is called `yourextractor`):

1. [Fork this repository](https://github.com/yt-dlp/yt-dlp/fork)
1. Check out the source code with:

        git clone git@github.com:YOUR_GITHUB_USERNAME/yt-dlp.git

1. Start a new git branch with

        cd yt-dlp
        git checkout -b yourextractor

1. Start with this simple template and save it to `yt_dlp/extractor/yourextractor.py`:

    ```python
    # coding: utf-8
    from .common import InfoExtractor
    
    
    class YourExtractorIE(InfoExtractor):
        _VALID_URL = r'https?://(?:www\.)?yourextractor\.com/watch/(?P<id>[0-9]+)'
        _TESTS = [{
            'url': 'https://yourextractor.com/watch/42',
            'md5': 'TODO: md5 sum of the first 10241 bytes of the video file (use --test)',
            'info_dict': {
                'id': '42',
                'ext': 'mp4',
                'title': 'Video title goes here',
                'thumbnail': r're:^https?://.*\.jpg$',
                # TODO more properties, either as:
                # * A value
                # * MD5 checksum; start the string with md5:
                # * A regular expression; start the string with re:
                # * Any Python type (for example int or float)
            }
        }]

        def _real_extract(self, url):
            video_id = self._match_id(url)
            webpage = self._download_webpage(url, video_id)
    
            # TODO more code goes here, for example ...
            title = self._html_search_regex(r'<h1>(.+?)</h1>', webpage, 'title')

            return {
                'id': video_id,
                'title': title,
                'description': self._og_search_description(webpage),
                'uploader': self._search_regex(r'<div[^>]+id="uploader"[^>]*>([^<]+)<', webpage, 'uploader', fatal=False),
                # TODO more properties (see yt_dlp/extractor/common.py)
            }
    ```
1. Add an import in [`yt_dlp/extractor/extractors.py`](yt_dlp/extractor/extractors.py).
1. Run `python test/test_download.py TestDownload.test_YourExtractor`. This *should fail* at first, but you can continually re-run it until you're done. If you decide to add more than one test, the tests will then be named `TestDownload.test_YourExtractor`, `TestDownload.test_YourExtractor_1`, `TestDownload.test_YourExtractor_2`, etc. Note that tests with `only_matching` key in test's dict are not counted in. You can also run all the tests in one go with `TestDownload.test_YourExtractor_all`
1. Make sure you have atleast one test for your extractor. Even if all videos covered by the extractor are expected to be inaccessible for automated testing, tests should still be added with a `skip` parameter indicating why the purticular test is disabled from running.
1. Have a look at [`yt_dlp/extractor/common.py`](yt_dlp/extractor/common.py) for possible helper methods and a [detailed description of what your extractor should and may return](yt_dlp/extractor/common.py#L91-L426). Add tests and code for as many as you want.
1. Make sure your code follows [yt-dlp coding conventions](#yt-dlp-coding-conventions) and check the code with [flake8](https://flake8.pycqa.org/en/latest/index.html#quickstart):

        $ flake8 yt_dlp/extractor/yourextractor.py

1. Make sure your code works under all [Python](https://www.python.org/) versions supported by yt-dlp, namely CPython and PyPy for Python 3.6 and above. Backward compatability is not required for even older versions of Python.
1. When the tests pass, [add](https://git-scm.com/docs/git-add) the new files, [commit](https://git-scm.com/docs/git-commit) them and [push](https://git-scm.com/docs/git-push) the result, like this:

        $ git add yt_dlp/extractor/extractors.py
        $ git add yt_dlp/extractor/yourextractor.py
        $ git commit -m '[yourextractor] Add extractor'
        $ git push origin yourextractor

1. Finally, [create a pull request](https://help.github.com/articles/creating-a-pull-request). We'll then review and merge it.

In any case, thank you very much for your contributions!


## yt-dlp coding conventions

This section introduces a guide lines for writing idiomatic, robust and future-proof extractor code.

Extractors are very fragile by nature since they depend on the layout of the source data provided by 3rd party media hosters out of your control and this layout tends to change. As an extractor implementer your task is not only to write code that will extract media links and metadata correctly but also to minimize dependency on the source's layout and even to make the code foresee potential future changes and be ready for that. This is important because it will allow the extractor not to break on minor layout changes thus keeping old yt-dlp versions working. Even though this breakage issue may be easily fixed by a new version of yt-dlp, this could take some time, during which the the extractor will remain broken.


### Mandatory and optional metafields

For extraction to work yt-dlp relies on metadata your extractor extracts and provides to yt-dlp expressed by an [information dictionary](yt_dlp/extractor/common.py#L91-L426) or simply *info dict*. Only the following meta fields in the *info dict* are considered mandatory for a successful extraction process by yt-dlp:

 - `id` (media identifier)
 - `title` (media title)
 - `url` (media download URL) or `formats`

The aforementioned metafields are the critical data that the extraction does not make any sense without and if any of them fail to be extracted then the extractor is considered completely broken. While, in fact, only `id` is technically mandatory, due to compatability reasons, yt-dlp also treats `title` as mandatory. The extractor is allowed to return the info dict without url or formats in some special cases if it allows the user to extract usefull information with `--ignore-no-formats-error` - Eg: when the video is a live stream that has not started yet.

[Any field](yt_dlp/extractor/common.py#219-L426) apart from the aforementioned ones are considered **optional**. That means that extraction should be **tolerant** to situations when sources for these fields can potentially be unavailable (even if they are always available at the moment) and **future-proof** in order not to break the extraction of general purpose mandatory fields.

#### Example

Say you have some source dictionary `meta` that you've fetched as JSON with HTTP request and it has a key `summary`:

```python
meta = self._download_json(url, video_id)
```
    
Assume at this point `meta`'s layout is:

```python
{
    "summary": "some fancy summary text",
    "user": {
        "name": "uploader name"
    },
    ...
}
```

Assume you want to extract `summary` and put it into the resulting info dict as `description`. Since `description` is an optional meta field you should be ready that this key may be missing from the `meta` dict, so that you should extract it like:

```python
description = meta.get('summary')  # correct
```

and not like:

```python
description = meta['summary']  # incorrect
```

The latter will break extraction process with `KeyError` if `summary` disappears from `meta` at some later time but with the former approach extraction will just go ahead with `description` set to `None` which is perfectly fine (remember `None` is equivalent to the absence of data).


If the data is nested, do not use `.get` chains, but instead make use of the utility functions `try_get` or `traverse_obj`

Considering the above `meta` again, assume you want to extract `["user"]["name"]` and put it in the resulting info dict as `uploader`

```python
uploader = try_get(meta, lambda x: x['user']['name'])  # correct
```
or
```python
uploader = traverse_obj(meta, ('user', 'name'))  # correct
```

and not like:

```python
uploader = meta['user']['name']  # incorrect
```
or
```python
uploader = meta.get('user', {}).get('name')  # incorrect
```


Similarly, you should pass `fatal=False` when extracting optional data from a webpage with `_search_regex`, `_html_search_regex` or similar methods, for instance:

```python
description = self._search_regex(
    r'<span[^>]+id="title"[^>]*>([^<]+)<',
    webpage, 'description', fatal=False)
```

With `fatal` set to `False` if `_search_regex` fails to extract `description` it will emit a warning and continue extraction.

You can also pass `default=<some fallback value>`, for example:

```python
description = self._search_regex(
    r'<span[^>]+id="title"[^>]*>([^<]+)<',
    webpage, 'description', default=None)
```

On failure this code will silently continue the extraction with `description` set to `None`. That is useful for metafields that may or may not be present.


Another thing to remember is not to try to iterate over `None`

Say you extracted a list of thumbnails into `thumbnail_data` using `try_get` and now want to iterate over them

```python
thumbnail_data = try_get(...)
thumbnails = [{
    'url': item['url']
} for item in thumbnail_data or []]  # correct
```

and not like:

```python
thumbnail_data = try_get(...)
thumbnails = [{
    'url': item['url']
} for item in thumbnail_data]  # incorrect
```

In the later case, `thumbnail_data` will be `None` if the field was not found and this will cause the loop `for item in thumbnail_data` to raise a fatal error. Using `for item in thumbnail_data or []` avoids this error and results in setting an empty list in `thumbnails` instead.


### Provide fallbacks

When extracting metadata try to do so from multiple sources. For example if `title` is present in several places, try extracting from at least some of them. This makes it more future-proof in case some of the sources become unavailable.


#### Example

Say `meta` from the previous example has a `title` and you are about to extract it. Since `title` is a mandatory meta field you should end up with something like:

```python
title = meta['title']
```

If `title` disappears from `meta` in future due to some changes on the hoster's side the extraction would fail since `title` is mandatory. That's expected.

Assume that you have some another source you can extract `title` from, for example `og:title` HTML meta of a `webpage`. In this case you can provide a fallback scenario:

```python
title = meta.get('title') or self._og_search_title(webpage)
```

This code will try to extract from `meta` first and if it fails it will try extracting `og:title` from a `webpage`.


### Regular expressions

#### Don't capture groups you don't use

Capturing group must be an indication that it's used somewhere in the code. Any group that is not used must be non capturing.

##### Example

Don't capture id attribute name here since you can't use it for anything anyway.

Correct:

```python
r'(?:id|ID)=(?P<id>\d+)'
```

Incorrect:
```python
r'(id|ID)=(?P<id>\d+)'
```

#### Make regular expressions relaxed and flexible

When using regular expressions try to write them fuzzy, relaxed and flexible, skipping insignificant parts that are more likely to change, allowing both single and double quotes for quoted values and so on.

##### Example

Say you need to extract `title` from the following HTML code:

```html
<span style="position: absolute; left: 910px; width: 90px; float: right; z-index: 9999;" class="title">some fancy title</span>
```

The code for that task should look similar to:

```python
title = self._search_regex(  # correct
    r'<span[^>]+class="title"[^>]*>([^<]+)', webpage, 'title')
```

Or even better:

```python
title = self._search_regex(  # correct
    r'<span[^>]+class=(["\'])title\1[^>]*>(?P<title>[^<]+)',
    webpage, 'title', group='title')
```

Note how you tolerate potential changes in the `style` attribute's value or switch from using double quotes to single for `class` attribute: 

The code definitely should not look like:

```python
title = self._search_regex(  # incorrect
    r'<span style="position: absolute; left: 910px; width: 90px; float: right; z-index: 9999;" class="title">(.*?)</span>',
    webpage, 'title', group='title')
```

or even

```python
title = self._search_regex(  # incorrect
    r'<span style=".*?" class="title">(.*?)</span>',
    webpage, 'title', group='title')
```

Here the presence or absence of other attributes including `style` is irrelevent for the data we need, and so the regex must not depend on it


### Long lines policy

There is a soft limit to keep lines of code under 100 characters long. This means it should be respected if possible and if it does not make readability and code maintenance worse. Sometimes, it may be reasonable to go upto 120 characters and sometimes even 80 can be unreadable. Keep in mind that this is not a hard limit and is just one of many tools to make the code more readable

For example, you should **never** split long string literals like URLs or some other often copied entities over multiple lines to fit this limit:

Correct:

```python
'https://www.youtube.com/watch?v=FqZTN594JQw&list=PLMYEtVRpaqY00V9W81Cwmzp6N6vZqfUKD4'
```

Incorrect:

```python
'https://www.youtube.com/watch?v=FqZTN594JQw&list='
'PLMYEtVRpaqY00V9W81Cwmzp6N6vZqfUKD4'
```

### Inline values

Extracting variables is acceptable for reducing code duplication and improving readability of complex expressions. However, you should avoid extracting variables used only once and moving them to opposite parts of the extractor file, which makes reading the linear flow difficult.

#### Example

Correct:

```python
title = self._html_search_regex(r'<title>([^<]+)</title>', webpage, 'title')
```

Incorrect:

```python
TITLE_RE = r'<title>([^<]+)</title>'
# ...some lines of code...
title = self._html_search_regex(TITLE_RE, webpage, 'title')
```


### Collapse fallbacks

Multiple fallback values can quickly become unwieldy. Collapse multiple fallback values into a single expression via a list of patterns.

#### Example

Good:

```python
description = self._html_search_meta(
    ['og:description', 'description', 'twitter:description'],
    webpage, 'description', default=None)
```

Unwieldy:

```python
description = (
    self._og_search_description(webpage, default=None)
    or self._html_search_meta('description', webpage, default=None)
    or self._html_search_meta('twitter:description', webpage, default=None))
```

Methods supporting list of patterns are: `_search_regex`, `_html_search_regex`, `_og_search_property`, `_html_search_meta`.


### Trailing parentheses

Always move trailing parentheses after the last argument.

Note that this *does not* apply to braces `}` or square brackets `]` both of which should closed be in a new line

#### Example

Correct:

```python
    lambda x: x['ResultSet']['Result'][0]['VideoUrlSet']['VideoUrl'],
    list)
```

Incorrect:

```python
    lambda x: x['ResultSet']['Result'][0]['VideoUrlSet']['VideoUrl'],
    list,
)
```


### Use convenience conversion and parsing functions

Wrap all extracted numeric data into safe functions from [`yt_dlp/utils.py`](yt_dlp/utils.py): `int_or_none`, `float_or_none`. Use them for string to number conversions as well.

Use `url_or_none` for safe URL processing.

Use `try_get`, `dict_get` and `traverse_obj` for safe metadata extraction from parsed JSON.

Use `unified_strdate` for uniform `upload_date` or any `YYYYMMDD` meta field extraction, `unified_timestamp` for uniform `timestamp` extraction, `parse_filesize` for `filesize` extraction, `parse_count` for count meta fields extraction, `parse_resolution`, `parse_duration` for `duration` extraction, `parse_age_limit` for `age_limit` extraction. 

Explore [`yt_dlp/utils.py`](yt_dlp/utils.py) for more useful convenience functions.

#### More examples

##### Safely extract optional description from parsed JSON
```python
description = traverse_obj(response, ('result', 'video', 'summary'), expected_type=str)
```

##### Safely extract more optional metadata
```python
video = traverse_obj(response, ('result', 'video', 0), default={}, expected_type=dict)
description = video.get('summary')
duration = float_or_none(video.get('durationMs'), scale=1000)
view_count = int_or_none(video.get('views'))
```




# EMBEDDING YT-DLP
See [README.md#embedding-yt-dlp](README.md#embedding-yt-dlp) for instructions on how to embed yt-dlp in another Python program
