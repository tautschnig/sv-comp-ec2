# Script to automate performance evaluation of CBMC on AWS

Evaluation is performed for a chosen GitHub or CodeCommit repository and
commit/branch/tag by running (a subset of) SV-COMP benchmarks. Runs are
done both in optimised as well as in profiling mode.

Execution should be as simple as

```
./perf_test.py \
			-r https://github.com/diffblue/cbmc -c develop \
			-e your@email.address
```

assuming that AWS access keys are set up (as required for AWS cli use) and boto3
is installed. Emails will then be sent as results become available.

The script sets up (and persists) an S3 bucket for storing results, SNS
queues for email updates on the process, as well as an EBS snapshot
containing the benmarking data.

For each benchmarking run, builds are set up and performed via
CodeBuild. Evaluation is then performed using AutoScalingGroups
synchronised via SQS.

The design premise for this work was:

"Compare multiple configurations for performance, correctness, capabilities."

This was broken down into:

Configuration: target platform, timeout, memory limit, benchmark set, tool
options, source version (=repository + revision).

Compare: log files, counterexamples, CPU profile, memory profile, verification
results.
