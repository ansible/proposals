# The Ansible Proposal process

The Core committee is responsible for evolving Ansible (the language), and authoring the specification. 
The committee operates by consensus and has discretion to alter the specification as it sees fit. 
However, the general process for making changes to the specification is as follows:

## Development
Changes to Ansible are developed by way of a process which provides guidelines for evolving an addition from an idea to a fully specified feature, complete with acceptance tests and multiple implementations. 
There are four "maturity" stages. The Core committee should approve acceptance for each stage.

<table>
  <caption>Maturity Stages</caption>
  <thead>
    <tr>
      <th>
      <th>Stage
      <th>Purpose
      <th>Entrance Criteria
      <th>Acceptance Signifies
      <th>Spec Quality
      <th>Post-Acceptance Changes Expected
      <th>Implementation Types Expected*
    </tr>
  </thead>
  <tr>
    <td>0
    <td>BrainDump vs LightBulb
    <td>Allow input into the specification
    <td>We'd like to see an issue created in our proposals repo
    <td>N/A
    <td>N/A
    <td>N/A
    <td>N/A
  </tr>
  <tr>
    <td>1
    <td>Proposal
    <td>
      <ul>
        <li>Make the case for the addition
        <li>Describe the shape of a solution
        <li>Identify potential challenges
      </ul>
    </td>
    <td>
      <ul>
        <li>Identified “Champion” who will advance the addition
        <li>Prose outlining the problem or need and the general shape of a solution
        <li>Illustrative examples of usage
        <li>High-level API
        <li>Discussion of key algorithms, abstractions and semantics
        <li>Identification of potential “cross-cutting” concerns and implementation challenges/complexity
      </ul>
    </td>
    <td>The committee expects to devote time to examining the problem space, solutions and cross-cutting concerns
    </td>
    <td>None
    <td>Major
    <td>Polyfills / demos
  </tr>
  <tr>
    <td>2
    <td>Draft
    <td>Describe the syntax and semantics using formal spec language
    <td>
      <ul>
        <li>Above
        <li>Initial spec text
      </ul>
    </td>
    <td>The committee expects the feature to be developed and eventually included in the standard deployment
    <td>Draft: all <em>major</em> semantics, syntax and API are covered, but TODOs, placeholders and editorial issues are expected
    <td>Incremental
    <td>Experimental
  </tr>
  <tr>
    <td>3
    <td>Candidate
    <td>Indicate that further refinement will require feedback from implementations and users
    <td>
      <ul>
        <li>Above
        <li>Complete spec text
        <li>Designated reviewers have accepted the current proposal text
        <li>The Core committee have accepted the current proposal text
      </ul>
    </td>
    <td>The solution is complete and no further work is possible without implementation experience, significant usage and external feedback.
    <td>Complete: all semantics, syntax and API are completed described
    <td>Limited: only those deemed critical based on implementation experience
    <td>Spec compliant
  </tr>
  <tr>
    <td>4
    <td>Finished
    <td>Indicate that the addition is ready for inclusion in the formal/upstream Ansible
    <td>
      <ul>
        <li>Above
        <li>Acceptance tests have been written for mainline usage scenarios, and merged
        <li>Two compatible implementations which pass the acceptance tests
        <li>Significant in-the-field experience with shipping implementations, such as that provided by two independent VMs
        <li>A pull request has been sent to <a href="https://github.com/ansible/ansible">ansible/ansible</a> with the integration
        <li>The Ansible core team have sight of the pull request, by it being added to a core meeting
        <li> The Identified “champion” or one of the community member should attend the core meenting where pull request is discussed.
      </ul>
    </td>
    <td>The addition will be included in the soonest practical standard revision
    <td>Final: All changes as a result of implementation experience are integrated
    <td>None
    <td>Shipping
  </tr>
</table>

## Definitions

- Champion: The person who is responsible for driving the proposal through all stages, until completion.
This may also include future reviews to the proposal, or what it achieves 

## Input into the process

Ideas for evolving Ansible are accepted in any form. 
Any discussion, idea or proposal for a change or addition which has not been submitted as a formal proposal is considered to be a “BrainDump vs LightBulb” (stage 0) and has no acceptance requirements.

# Status of in-process additions

Ansible Core will maintain a list of in-process additions, along with the current maturity stage of each, on its GitHub.

# Spec text

At stages “draft” (stage 2) and later, the semantics, API and syntax of an addition should be described as edits to the latest published standard, using the same language and conventions. The quality of the spec text expected at each stage is described above.

# Reviewers

Anyone can be a reviewer and submit feedback on an in-process addition. The committee should identify designated reviewers for acceptance during the “draft” (stage 2) maturity stage. 
These reviewers must give their sign-off / acceptance before a proposal enters the “candidate” (stage 3) maturity stage. Designated reviewers should not be authors of the spec text for the addition and should have expertise applicable to the subject matter. Designated reviewers must be chosen by the committee, not by the proposal's champion.

When reviewers are designated, a target meeting for Stage 3 should be identified. Initial reviewer feedback should be given to the champions two weeks before that meeting to allow for a back-and-forth ahead of the meeting. The target Stage 3 meeting may be delayed by a champion outside of the meeting at a later time if it is not ready.

# Calls for implementation and feedback

When an addition is accepted at the “candidate” (stage 3) maturity level, the committee is signifying that it believes design work is complete and further refinement will require implementation experience, significant usage and external feedback.

# Tests

During stage 3, tests should be authored and submitted via pull request. Once it has been appropriately reviewed, it should be merged to aid implementors in providing the feedback expected during this stage.

# Eliding the process

The committee may elide the process based on the scope of a change under consideration as it sees fit.

# Role of the editor

In-process additions will likely have spec text which is authored by a champion or a committee member other than the editor although in some case the editor may also be a champion with responsibility for specific features. The editor is responsible for the overall structure and coherence of the proposed implementation. It is also the role of the editor to provide guidance and feedback to spec text authors so that as an addition matures, the quality and completeness of its specification improves. It is also the role of the editor to integrate additions which have been accepted as “finished” (stage 4) into the a new revision of the specification.
