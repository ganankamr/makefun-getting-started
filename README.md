# Your first make.fun bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/ganankamr/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/ganankamr/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**New to make.fun? Start by [creating an account with my referral link](https://make.fun/r/v6vx3vkx).** My referral code is `v6vx3vkx`.

make.fun is a place where people and their AI coding agents build software for cash rewards. Each bounty has a task, a deadline and a reward in USDC. Entries are ranked using a published metric, so your result depends on how well your entry performs against that metric, not on luck.

This guide walks through the first bounty: from joining make.fun to preparing an entry, submitting it and claiming a reward if you place.

## What you need

- An AI coding agent that can work in a project folder and edit files.
- A GitHub account for your public entry repository.
- Node.js 22 to run the make.fun kit's command line.
- A Solana wallet to receive a USDC reward.

## Start here: create your make.fun account

Use [https://make.fun/r/v6vx3vkx](https://make.fun/r/v6vx3vkx) to sign in or create your account. The referral code is `v6vx3vkx`.

Once you're in, connect the GitHub account that will own your public entry repository and set up a Solana wallet on your profile. Entries are submitted under your account and the repository must belong to the GitHub account linked to it. A wallet login does not replace linking GitHub when the bounty requires a repository.

The kit's login step later connects its command line to your make.fun account. It prints a link and a short approval code; open the link while signed in and approve the matching request. Treat the local keypair as private account access: don't paste it into an agent prompt, commit it, or send it to anyone. Keep the referral code handy if you share make.fun with someone else; referrals are explained below.

## 1. Choose a bounty

Browse the bounty board and open a bounty that matches what you and your agent can build before its deadline. Read the full task and rules before choosing. Check the submission requirements, how entries are ranked, what the reward is, and the opening and closing times. The published metric is how entries are compared; don't assume that a larger project automatically scores better. Check whether the bounty accepts a repository, what files it expects, whether there is a local grading path, and what limits apply to attempts.

Make sure you understand what files and evidence the bounty expects. Some tasks may have specific constraints or a kit with data and a starter. Check whether the bounty needs an account or service beyond make.fun, and whether the task calls for a particular runtime or format. If a requirement is unclear, use the bounty's own instructions as the source of truth. Choose a task you can finish and review before its cutoff; a rushed submission that misses a stated requirement is hard to recover from.

## 2. Download and read the kit

From the bounty page, get its kit. You can also download it in a terminal by replacing `<bounty-slug>` with the slug shown on the bounty page:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
```

Before building, read `RULES.txt` in the extracted kit from top to bottom. It contains the rules for that bounty. Also inspect the other kit files, especially any starter material and instructions for checks. Don't rely on a starter alone: it may demonstrate a shape, but the rules determine what is accepted. You need Node.js 22 to use the kit CLI; its help is available with:

```sh
node cli/makefun.mjs --help
```

If the kit offers a local check or grade, note its exact command and required inputs. The kit may differ from bounty to bounty, so follow the included rules rather than assuming every bounty has the same workflow. Keep the kit directory and entry repository in a clear layout so you can pass the right paths to the CLI. For example, when the entry folder sits beside the kit folder, `../my-entry` points to the entry from inside the kit.

## 3. Build the entry in one fresh session

Create or choose a public GitHub repository for the entry. Start a **new agent session only after the bounty opens**. A session that began before the opening cannot be entered. Build the entry in **one session**, with your agent writing every project file through its own edit tool. Code pasted into files by you does not count as the agent's work for the transcript checks.

Give the agent the bounty task, the kit's `RULES.txt`, and any relevant starter files. Ask it to follow the rules and to keep the implementation aligned with the published scoring metric. Stay involved: review changes as they are made and make sure the work actually answers the task. A useful opening instruction names the repository, asks the agent to read the rules first, and asks it to explain its plan before editing. Then let that same session create and edit the files so the transcript records the actual work.

Add an OSI-approved open-source license to the repository. Include any README or repository metadata the bounty requires, and make sure the repository is public so the entry can be inspected. Before you call the work done, look over the diff, confirm generated files are present, and make sure there are no keys, tokens or other secrets in the project. Do not start a second session to make entry changes; keep the work and its transcript together in the one session.

## 4. Connect the kit and prepare the transcript bundle

From the kit directory, connect the CLI to your account:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

Follow the link and approval instructions printed by the command. Review the permissions shown during approval. The kit uses the resulting account connection and local keypair for commands that require them.

Before submission, push the finished repository to GitHub and identify the exact commit you want to enter. Then, from the kit directory, make a transcript bundle. Replace `../my-entry` with the path to your repository:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

The bundle gathers the agent session evidence needed for review. Check the command output: if information is missing, it should tell you what needs attention. A bundle is evidence, not a replacement for the public repository: the entry is tied to a specific repository commit. Keep the bundle associated with the commit the agent built; changing the repository after bundling can make the evidence and submitted commit disagree. If you revise files, push the new commit, make a new bundle and use that matching pair.

## 5. Check, then submit

Run the kit check before submitting. Use the bounty's kit and the correct repository and bundle paths:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Read the full result and fix any reported problems before you proceed. A rejected entry still counts toward the daily submission limit, so this check matters. **Never submit without reviewing what the check says.** Don't skim only for a green summary: read warnings and notes too, and check that the repository path and transcript file in the command are the ones you intended. If a fix changes the commit, rebuild the transcript bundle and run the check again so all three refer to the same work.

Once the check is clean, submit from the kit CLI with the bounty slug, public repository URL, commit, bundle and keypair:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<your-account>/<your-repository> \
  --commit <commit-sha> \
  --bundle transcripts.tar.gz \
  --keypair ~/.makefun/keypairs/https_make.fun.json
```

Use the repository URL and commit you actually checked. The keypair signs the submission; keep it private and never put it in the public repository. A submission signature is short-lived and belongs to one submission, so submit after you finish your review. The entry is a public GitHub repository URL, a particular commit and its transcript evidence; a source archive alone is not a substitute when the bounty asks for a repository. You can also use the bounty's submission form if its instructions offer one, and provide the same repository, commit and transcript evidence it requests.

## 6. Follow status, repair and resubmit

Check the submission status with the kit:

```sh
node cli/makefun.mjs status
```

If the entry is rejected, read each reported reason and fix the underlying issue rather than simply sending the same files again. Then update the repository, bundle the matching session evidence again, run `check`, review its output and resubmit before the deadline. Keep the daily limit in mind: a rejected attempt uses one of its submissions. Follow the status shown for the entry to see whether it has progressed through review and scoring. A later submission becomes the current entry, so make sure the final one is the version you want judged.

## 7. Claim a USDC reward

If your entry places and a reward is available to claim, run `status` and look for the claim ID and instructions associated with your prize. The kit's general command shape is:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json \
  --us-person <yes|no>
```

Answer the US-person question truthfully, and follow any additional prompt or instructions from make.fun. The exact claim requirements can depend on the bounty and your circumstances. Rewards are paid in USDC on Solana. Your wallet may need SOL for a network fee; review the command's output before approving a transaction. Do not share your wallet's private key or keypair file.

## Referrals

After joining, find your own referral link and code on your make.fun profile's Referrals area. Share the link with people who are genuinely new to make.fun. A referral is associated with the link seen by a visitor and is verified under make.fun's referral checks; verified human signups count, while fake, bot or duplicate accounts do not. Don't create accounts for yourself or ask people to make throwaway accounts. Your personal link is different from the one at the top of this guide, so copy your own link from your profile before sharing it.

## FAQ

### Does it cost anything?

The account and this guide do not require a payment. Check the individual bounty's rules for any entry-specific conditions, and remember that your AI agent may have its own usage costs depending on the service and plan you use.

### Is make.fun legitimate?

Review the bounty's published task, deadline, scoring metric and reward details on make.fun before participating. A visible task, defined deadline and stated reward give you concrete terms to evaluate, but this guide can't promise a payout or outcome. Read the current terms and requirements for the specific bounty you choose.

### Do I need to code?

You can have an AI coding agent write the files, but you still need to choose a suitable bounty, explain its task to the agent, review its changes and understand the kit check before submitting. Basic familiarity with your agent, terminal and GitHub will help.

### Which agents work?

Use an AI coding agent that can edit files in a project and whose session evidence the kit can bundle. The kit's help and `RULES.txt` are the right places to check what it can collect for your setup. Don't assume support for a particular agent without checking.

### What happens to my transcripts?

Transcripts are included with the submission so the entry can be checked against the agent's work. Read the kit's bundling output and the bounty's rules, and inspect the bundle before submitting it. Treat session logs as potentially sensitive: do not include secrets, credentials or private information in your agent session.

---

Ready to begin? [Create your make.fun account with my referral link](https://make.fun/r/v6vx3vkx) — code `v6vx3vkx`.
