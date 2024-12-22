---
name: Release task
about: Create a release task
title: "[RELEASE]"
labels: release/task
assignees: ''

---

## What's the task? Please describe.
Action items for releasing v<x.y.z>

## Roles
- Release captain: <!--responsible for RD efforts of release development and coordinating with QA captain-->
- QA captain: <!--responsible for coordinating QA efforts of release testing tasks-->

## Describe the sub-tasks.

### Pre-Release

**The Release Captain needs to finish the following items.**

- [ ] This tasks are only needed when doing a feature release such as x.y.0)
  - [ ] Before creating RC1, create a new release branch for the following component repositories, and then create RC1 from the new branch. Leave the master branch for the next feature release development.
    - longhorn-manager
    - longhorn-ui
    - longhorn-tests
    - longhorn-engine
    - longhorn-instance-manager
    - longhorn-share-manager
    - backing-image-manager
    - longhorn-spdk-engine (needed after GA)
    - cli
  - [ ] Add the new branch (x.y.0) to renovate configuration in https://github.com/longhorn/release/blob/main/renovate-default.json.
- [ ] Update image versions in https://github.com/longhorn/longhorn/tree/v<x.y.z>/chart/README.md.
- [ ] Trigger the RC release build by [longhorn/release-preview Action](https://github.com/longhorn/release/actions/workflows/release-preview.yml)
- [ ] Trigger the GA release build by [longhorn/release Action](https://github.com/longhorn/release/actions/workflows/release.yml)

**The QA captain needs to coordinate the following items before the GA release.**

- [ ] Update `Best Practices>Operating System` in Longhorn official document.
- [ ] Regression test plan (manual)
- [ ] Run e2e regression for pre-GA milestones (`install`, `upgrade`) 
- [ ] Run security testing of container images for pre-GA milestones
  - [] Create security issues at upstream for unresolved CVEs in CSI sidecar images
- [ ] Verify longhorn chart PR to ensure all artifacts are ready for GA (`install`, `upgrade`)
- [ ] Run core testing (install, upgrade) for the GA build
  - Upgrade from the previous patch of the same feature release. 
  - Upgrade from the last patch of the previous feature release.
 
### Release

**The Release Captain needs to finish the following items.**

- [ ] Release note - Release Captain
  - [ ] Deprecation note
  - [ ] Upgrade notes including highlighted notes, deprecation, compatible changes, and others impacting the current users
- [ ] Publish the new version of doc and add a next patch version of dev doc.
  - [ ] Update image versions in https://longhorn.io `References > Helm Values` 
  - [ ] Update image versions in https://longhorn.io `Snapshot and Backups > CSI Snapshot Support > Enable CSI Snapshot Support on a Cluster`
- [ ] Release longhorn/chart from the release branch to publish to [ArtifactHub](https://artifacthub.io/packages/helm/longhorn/longhorn) by [longhorn/charts Actions](https://github.com/longhorn/charts)
- [ ] Mark the release as `latest` release in longhorn/longhorn [README.md](https://github.com/longhorn/longhorn)


### Post-Release

- [ ] Mark the release as `stable` release and update stable versions in https://github.com/longhorn/longhorn/blob/master/support-versions.txt
  - For the first stable release, we need to consider several factors and reach a consensus by maintainers before claiming it stable. 
  - For any patch release after a stable release, we need to wait 1-2 weeks for user feedback.

**After marking the release as a `stable` release, Release Captain needs to coordinate the following items**

- [ ] Update https://github.com/longhorn/longhorn/blob/master/deploy/upgrade_responder_server/chart-values.yaml  - @PhanLe1010 
- [ ] Add another request for the rancher charts for the next patch release - @rebeccazzzz  
- [ ] Update the [support matrix](https://www.suse.com/suse-longhorn/support-matrix/) - @asettle

### Rancher Charts

**The Release Captain needs to coordinate the following items.**

- [ ] Verify the chart can be installed & upgraded - QA captain
- [ ] rancher/image-mirrors update - @mantissahz @PhanLe1010
- [ ] rancher/charts active branches for Rancher App Marketplace - @mantissahz @PhanLe1010 

cc @longhorn/qa @longhorn/dev 
