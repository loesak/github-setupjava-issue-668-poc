# github-setupjava-issue-668-poc
https://github.com/actions/setup-java/issues/668

This repository is a demonstration of the issue identified in the above linked github issue.

# Scenarios

# Branch 'main'

The code in this branch demonstrates a working build. It is using the workaround identified where using the maven gpg plugin default environment variable name of `MAVEN_GPG_PASSPHRASE` to hold the value of the GPG passphrase in order to sign the build artifacts.

Also note that the debug output for the maven gpg plugin already includes the argument `--pinentry-mode loopback` in the gpg signing command despite the loopback configuration being missing in the maven gpg plugin, counter to github documentation. See line 2125 of the build output.

You can view the contents of this branch here:
https://github.com/loesak/github-setupjava-issue-668-poc

Here is the associated successful build:
https://github.com/loesak/github-setupjava-issue-668-poc/actions/runs/10706333315

# Branch 'workaround-specify-non-default-environment-variable-name'

The code in this branch demonstrates a working build. It is using the workaround identified where an environment variable name for the GPG passphrase that is not the maven gpg plugin default environment variable name is desired. The environment variable name is specified in both the GitHub workflow file and in the pom.xml file.

Also note that the debug output for the maven gpg plugin already includes the argument `--pinentry-mode loopback` in the gpg signing command despite the loopback configuration being missing in the maven gpg plugin, counter to github documentation. See line 2125 of the build output.

You can compare the changes to the main branch in this pull request:
https://github.com/loesak/github-setupjava-issue-668-poc/pull/1

And here is the associated successful build:
https://github.com/loesak/github-setupjava-issue-668-poc/actions/runs/10705963444

# Branch 'broken-use-alternative-environment-variable-name-no-loopback'

The code in this branch demonstrates the initial issue that worked with earlier versions of the maven gpg plugin. Here an alternative environment variable name is provided for the gpg passphrase without specifying the environment variable name in the maven gpg plugin configuration. This also does not include the pinentry loopback configuration. The build fails because the maven gpg plugin cannot find the gpg passphrase to use for artifact signing.

Also note that the debug output for the maven gpg plugin already includes the argument `--pinentry-mode error` in the gpg signing command. See line 2058 of the build output.

You can compare the changes to the main branch in this pull request:
https://github.com/loesak/github-setupjava-issue-668-poc/pull/2

And here is the associated successful build:
https://github.com/loesak/github-setupjava-issue-668-poc/actions/runs/10706079670

# Branch 'broken-use-alternative-environment-variable-name-yes-loopback'

The code in this branch is the same as the `broken-use-alternative-environment-variable-name-no-loopback` branch but adds the pinentry loopback configuration as [documented by GitHub](https://github.com/actions/setup-java/blob/main/docs/advanced-usage.md#extra-setup-for-pomxml). The result is the same however in that the build fails to sign the artifacts because it cannot find the gpg passphrase.

Also note that the debug output for the maven gpg plugin now includes two arguments for `--pinentry-mode` (`--pinentry-mode loopback` and `--pinentry-mode error`). I believe the command does not support multiple values and received the second flag `--pinentry-mode error` as the set value, thus ignoring the pinentry configuration provided in the maven gpg plugin. See line 2063 of the build output.

You can compare the changes to the main branch in this pull request:
https://github.com/loesak/github-setupjava-issue-668-poc/pull/3

And here is the associated successful build:
https://github.com/loesak/github-setupjava-issue-668-poc/actions/runs/10706247055