# Zotero Scholar Citations (ZSC)
This is an add-on for Zotero, a research source management tool. The add-on automatically fetches the number of citations for Zotero items from Google Scholar. It adds them to the 'Extra' field in Zotero and allows you to sort by number of citations. 

It searches for the number of citations when you add the paper or when you select "Update selections" for a Zotero item. It also allows (limited) batch updating the citations.

## Why a new fork?

This is a fork of the depreciated https://github.com/MaxKuehn/zotero-scholar-citations which is itself a fork of https://github.com/beloglazov/zotero-scholar-citations. 

This fork removes additional text in the citation field so that it's simply an integer. This makes it easier to sort by citation counts and takes up less room.

The previous fork was depreciated in 2019. However, it still worked well for me! Where problems *can* occur is when doing large batches, because the Google API is quite restricted. However, it works well for getting the requests when the paper is originally downloaded or when making individual/ small batch requests. 

## Installation
The add-on supports Zotero Standalone. To install it:
1. Download the lastest version of the add-on from [the release page](https://github.com/smlum/zotero-scholar-citations/releases) by right clicking the ".xpi" file and selecting "Save Link As...".
2. In Zotero (Standalone) go to Tools -> Add-ons -> click the settings button in the top-right corner -> Install Add-on From File -> select the downloaded file and restart Zotero.

## Batch requests
When updating multiple citations in a batch, it may happen that citation queries are blocked by Google Scholar for multiple automated requests. If a blockage happens, the add-on opens a browser window and directs it to http://scholar.google.com/, where you should see a Captcha displayed by Google Scholar, which you need to enter to get unblocked and then re-try updating the citations. It may happen that Google Scholar displays a message like the following "We're sorry... but your computer or network may be sending automated queries. To protect our users, we can't process your request right now." In that case, the only solution is to wait for a while until Google unblocks you.

If you just straight up update your entire collection you're bound to run into a captcha. Once you do run into one, all following queued update request will fail and you will be prompted for each and every one of them. Consider updating your collection in batches of 5-10 items at a time.

🚨 If anyone is able to help to address this limitation that would represent a major improvement!

### Staleness
As of Version 2.0.2 items whose citation count could not be updated will be marked with a staleness counter `[s0]`, e.g. `ZSCC: 0000042[s0]`, to signal the user that ZSC was unable to update the number of citations. If ZSC continuesly fails to update an item, it increases the staleness count up to a maximum of 9 and then wraps around.

The format of the staleness counter allows you to search for items with stale citation data by entering `[s` in Zoteros search bar.

### Existing "Extra"-Column Content
ZSC will
- update legacy ZSC "extra"-content, i.e. 5 digit citation counts and "No Citation Data" entries
- respect content that is already in the "Extra"-field by simply prepending the citation count to any existing content
    - this allows you to sort by the extra field to easily get the most/least cited items

## Why is ZSC unable retrieve the citation count for item X?
The most likely culprit is that ZSC search is too precise :^). Some Items do not have as complete of an author list on google scholar as they have in Zotero.

Here's how you can find out weather that's the problem
1. in Zotero enable debug out `Help > Debug Output Logging > enable`
1. then open the debug console `Help > Debug Output Logging > View Output`
1. try to update the item again
1. look out for something like this `[scholar-citations] GET https://scholar.google.com/scholar?hl=en&as_q=THE_TITLE_OF_YOUR_ITEM&as_epq=&as_occt=title&num=1&as_sauthors=AUTHOR1+AUTHOR2+AUTHOR3`
1. Copy & Paste that search link into a web browser of your choice
1. try removing different authors from the search

One combination of authors will certainly yield the correct search.

You can also temporarly recreate that combination in Zotero. ZSC will then successfully query that item. Once you re-add the author however, updates will fail again. :(

Read about how the orginal add-on was made: http://blog.beloglazov.info/2009/10/zotero-citations-from-scholar-en.html

For another option that uses other APIs to fetch citation numbers see [zotero-citationcounts](https://github.com/eschnett/zotero-citationcounts). Also, see the [Official Zotero Plugins Page](https://www.zotero.org/support/plugins) for more Zotero plugins.

## Why the Fork

The original maintainer [Anton Beloglazov](https://github.com/beloglazov) seems semi-active.

[Texot](https://github.com/tete1030) fixed some stuff that needed fixing BADLY, that is

- Fix detection of google robot checking
- Show `No Citation Data` in failure cases instead of `00000`

**But there's more that should be done!**

# License

Copyright (C) 2011-2013 Anton Beloglazov

Distributed under the Mozilla Public License 2.0 (MPL).
