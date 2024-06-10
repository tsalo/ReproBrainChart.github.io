---
title: "How to Get Data"
excerpt: "Get Data"
layout: single
permalink: /docs/get_data

---

### Step 1: Install DataLad

RBC is accessible via `datalad`. Follow the instructions [here](https://www.datalad.org/#install)
to get it installed.

### Step 2: Pick a dataset to clone

You can find each of the unprocessed BIDS MRI data, processed functional and processed structural
derivatives in their corresponding git repositories.

The git repositories (you can find them all
[here](https://github.com/orgs/ReproBrainChart/repositories)) are consistently named such that:

 * If you're looking for BIDS MRI, the repo will be named `<study>_BIDS`
 * If you're looking for processed functional data, the repo will be named `<study>_CPAC`
 * If you're looking for processed structural MRI, the repo will be named `<study>_FreeSurfer`

where `<study>` is replaced with `HBN`, `NKI`, `PNC`, `BHRC` or `CCNP`.


### Step 3: Pick a QC threshold and release version to use

When you download RBC data at a specific QC threshold you can be sure that
any file you find in your downloaded data is usable for further analysis.
RBC releases data at 3 different QC thresholds:

 * `complete-pass`: Structural and BOLD data have passed QC
 * `complete-artifact`: Structural data can be "Pass" or "Artifact", BOLD is "Pass"
 * `warning-fail`: Contains all the RBC data including QC failures. This threshold
  is not recommended and you'll have to explain why you chose this threshold in
  any publication resulting from it.

Lastly, you'll need to pick a _version_ of the data to get. Each time RBC makes
changes to the data, it will be released with a new version number. By picking
a version number for your RBC-based study you can be sure that your input data
will never change. The current release of RBC is `0.1`.


#### Cloning

Getting data on to your system will involve running a command like this:

```bash
$ datalad clone \
    https://github.com/ReproBrainChart/<study>_<content>.git \
    -b <qc_threshold>-<version>
```

Suppose I'd like to get some processed structural data from PNC. I would replace `<study>`
with `PNC` and `<content>` with `FreeSurfer`. I only want to include data that has passed
QC. I want the most recent release, version `0.1`. My command would be:

```bash
$ datalad clone \
    https://github.com/ReproBrainChart/PNC_FreeSurfer.git \
    -b complete-pass-0.1
```

You will see some warnings such as these:

```
[INFO   ] Unable to parse git config from origin
[INFO   ] Remote origin does not have git-annex installed; setting annex-ignore
[INFO   ] This could be a problem with the git-annex installation on the remote. Please make sure that git-annex-shell is available in PATH when you ssh into the remote. Once you have fixed the git-annex installation, run: git annex enableremote origin
install(ok): /home/cieslakm/data/PNC_FreeSurfer (dataset)
```

but you now have a `DataLad` dataset with everything you need to look at some data!

#### Getting data

That command probably finished a lot faster than you were expecting: PNC FreeSurfer has 1592
subjects in it! But this is normal for `DataLad`. (Note that cloning the processed functional data (i.e., `CPAC` files) will take longer as `CPAC` functional outputs include more files compared to `FreeSurfer`.) You will see a `PNC_FreeSurfer` directory
that you can look around in and see all the files you might want to copy to your workspace.

For example, let's take a look at the data we might want from one of the PNC subjects:

```bash
$ cd PNC_FreeSurfer/freesurfer/sub-192413932
$ ls
sub-192413932_brainmeasures.json@  sub-192413932_fsaverage.tar.xz@
sub-192413932_brainmeasures.tsv@   sub-192413932_fsLR_den-164k.tar.xz@
sub-192413932_freesurfer.tar.xz@   sub-192413932_regionsurfacestats.tsv@
```

We know from the data dictionary that the tabular data is available in the
two TSV files, so let's get a copy of them that we can open:

```bash
$ datalad get *.tsv
get(ok): freesurfer/sub-192413932/sub-192413932_regionsurfacestats.tsv (file) [from output-storage...]
get(ok): freesurfer/sub-192413932/sub-192413932_brainmeasures.tsv (file) [from output-storage...]
action summary:
  get (ok: 2)
```

Shell glob patterns can be used to get whichever files you might need.



