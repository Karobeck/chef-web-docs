.. The contents of this file may be included in multiple topics (using the includes directive).
.. The contents of this file should be modified in a way that preserves its ability to appear in multiple topics.


Everything in the lab is managed by |chef| from the https://github.com/mattray/lab-repo repository. The operating systems for the machines are provisioned by the `pxe_dust <https://supermarket.chef.io/cookbooks/pxe_dust/versions/1.1.0>`__ cookbook, which uses a preseed file to put a minimal operating system in place via PXE. These machines are frequently re-installed and the process takes about 10 minutes.

