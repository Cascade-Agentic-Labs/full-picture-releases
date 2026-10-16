# Full Picture releases

The fingerprints of every file that [fullpicture.family](https://fullpicture.family) serves, one record per release, published here so anyone can check that the pages their browser loads are the ones we released.

Full Picture seals rating-form answers in the browser before they are sent; our server stores only sealed data it cannot open. The one way around that would be to change the page code itself. This repository is part of how we guard against that: the fingerprints live somewhere we do not control alone, and each record is public history that cannot be quietly rewritten.

## Check the live site yourself

Each `releases/<date>-<commit>/release.json` lists the SHA-256 of every served file. To compare the live site with the latest record:

```sh
python3 - releases/<date>-<commit>/release.json <<'PY'
import json, hashlib, sys, urllib.request
m = json.load(open(sys.argv[1]))["sha256"]
route = {"index.html": "", "r.html": "r/AAAAAAAAAAAAAAAAAAAAAA", "p.html": "p/AAAAAAAAAAAAAAAAAAAAAA"}
bad = 0
for f, h in m.items():
    url = "https://fullpicture.family/" + route.get(f, f)
    b = urllib.request.urlopen(urllib.request.Request(url, headers={"User-Agent": "fp-check"})).read()
    if hashlib.sha256(b).hexdigest() != h:
        bad += 1; print("DIFFERS", f)
print(len(m), "files,", bad, "differ")
PY
```

A clean result shows the files you fetched match the release. It cannot prove what another visitor was served.

## What a record holds

`released` (date), `commit`, `cloudRunRevision`, `image` (the container digest), and `sha256` (file to hash). Records are only added, never edited.

Cascade Agentic Labs LLC, 1710 N 46th St, Seattle, WA 98103 · security reports: support@fullpicture.family
