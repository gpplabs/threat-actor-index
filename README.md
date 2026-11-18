# Threat Actor Index

An index of the threat actor names that show up in public threat intelligence reporting, along with the aliases the reports themselves state. Every entry cites the report(s) it comes from.

Published by [GPP Labs](https://gpplabs.io). Updated monthly.

## Browse

- **[All names, A–Z](aliases.md):** start here if you have a name from a report, like "Fancy Bear" or "Storm-0539."
- **[All groups](actors/README.md):** every group in one table, with report counts and when each was last seen.
- **Search:** GitHub's search box finds a name on any page.

Each group has its own page with every name it goes by, how each name was tied to it, and the reports that named it, linked.

## What Is This?

If you've spent any time reading threat intel, you've run into this: the same group shows up under five or six names depending on who wrote the report. One vendor's TA422 is another's Forest Blizzard, and a government advisory might call it by a military unit number instead. This index is an attempt to keep those straight.

This project is an output of GPP Labs' CTI pipeline. For each group named, the index lists:

- **Names:** every name the reports used, and how each one was tied to the group.
- **Counts:** how many reports and publishers named the group, and when it was last seen.
- **Reports:** the reports themselves, by title and link.

Names are matched against MITRE ATT&CK® first, then the MISP threat-actor galaxy. The index also adds aliases the reports state themselves ("formerly tracked as," "also known as"), but only after each one has been checked against the source text and confirmed in review.

## What It's Not

- **Not attribution:** an entry means public reports named the group, nothing more. A report that mentions a group in passing still counts as naming it.
- **Not a catalog:** only groups named in the collected reporting show up, and only names that resolve to a known group or a confirmed alias.
- **Not a measure of activity:** counts reflect which publishers the pipeline collects from, not how busy a group is.

## How Are Names Resolved?

Each name in `data/names.*` has a `resolved_via` value:

- `MITRE ATT&CK`: the name is an ATT&CK group name or alias.
- `MISP primary name` or `MISP synonym`: the name is a MISP threat-actor entry's name or one of its synonyms. If the two catalogs disagree, ATT&CK wins.
- `stated in reporting`: a report states the alias itself. These names also carry a `basis`:
  - `authority`: the publisher renamed or graduated its own cluster, like GTIG's ARMORY CASTLE, formerly UNC5221.
  - `corroborated`: two independent publishers state it. News sites and aggregators that repeat a vendor's reporting never count as the second.
  - `reviewed`: the statement was checked against its source and confirmed in review.

A few rules sit behind that:

- **Entries follow the operator:** when reporting ties a public persona to the unit behind it, the persona's name is listed under that unit, the same way ATT&CK and MISP treat them. Mr. Soul and Soldiers of Solomon show up under CyberAv3ngers, for example.
- **Overlap isn't identity:** a report saying two groups "overlap," or two vendors attributing the same incident to their own names, doesn't merge anything.
- **Umbrellas stay whole:** some MISP entries span more than one ATT&CK group. Those are marked `umbrella`, with the groups they span, and nothing is merged into them.

## Keys

Every group has a permanent key:

- an ATT&CK group ID, like `G0007`;
- `misp:<uuid>` for a group only MISP knows;
- `reporting:<name>` for a group neither catalog has, known here only because reports state its aliases, like `reporting:cordialspider`.

Display names can change from month to month; keys don't. If a key is ever merged into another or withdrawn, `data/redirects.*` records where it went and why.

## Files

| File | What's in it |
|---|---|
| `data/actors.json`, `data/actors.csv` | One entry per group: key, display name, umbrella, report and publisher counts, and last seen. The JSON also carries each group's 10 most recent citations. |
| `data/names.json`, `data/names.csv` | One row per group and name, with how the name resolved. |
| `data/citations.json`, `data/citations.csv` | Every citing report: title, URL, publisher, publication date, and the names it used. |
| `data/redirects.json`, `data/redirects.csv` | Keys that were merged or withdrawn. These files show up once there are any. |
| `meta.json` | The export date, the corpus date range, the ATT&CK version, the MISP commit, and totals. |

Every row carries `license`, `source`, and `source_ref`, so you can tell where each piece came from. Citations are titles and links only; no report text is reproduced here.

## License

The license is marked per row:

- **`CC0-1.0`:** everything from MISP, and everything this index adds (stated aliases, counts, dates, and citations).
- **`LicenseRef-MITRE-ATTACK`:** rows that carry ATT&CK content, such as a group's ATT&CK name or aliases. These stay under MITRE's terms, which `NOTICE` reproduces. If you copy those rows, copy the notice with them.

MISP's galaxy is dual-licensed under CC0 or BSD 2-Clause; this index uses it under CC0, with thanks to the MISP project and its contributors.

This project isn't affiliated with or endorsed by MITRE. MITRE ATT&CK® and ATT&CK® are registered trademarks of The MITRE Corporation.

## Citing It

Cite the release you used, for example:

> GPP Labs. *Threat Actor Index*, release 2026.11. https://github.com/gpplabs/threat-actor-index

## Updates and Corrections

A new release goes out about once a month, tagged by year and month. Each one is reviewed before it's published, including everything added, removed, merged, or renamed since the last.

If you find a name that's attached to the wrong group, or a stated alias that doesn't match its source, please open an issue and include the report that shows it. Corrections are welcome.

The pipeline behind the index is not public.
