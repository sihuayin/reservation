# Core Resercation

- Feature Name: core-reservation
- Start Date: 2024-03-14 14:51:33

## Summary

主要组件

## Motivation

需要一个通用的预定逻辑。
1. 会议预定
2. 房价预定
等

## Guide-level explanation

### service interface

```proto
enum ReservationStatus {
  UNKNOWN = 0;
  PENDING = 1;
  CONFIRMED = 2;
  BLOCKED = 3;
}
message Reservation {
  string id = 1;
  string user_id = 2;
  ReservationStatus status = 3;

  // resource reservation window
  string resource_id = 4;
  google.protobuf.Timestamp start = 5;
  google.protobuf.Timestamp end = 6;
  // extra note
  string note = 7;
}

message ReserveRequest {
  Reservation reservation = 1;
}

message ReserveResponse {
  Reservation reservation = 1;
}

message UpdateRequest {
  string note = 2;
}

message UpdateResponse {
  Reservation reservation = 1;
}

message ConfirmRequest {
  string id = 1;
}

message ConfirmResponse {
  Reservation reservation = 1;
}

message CancelRequest {
  string id = 1;
}

message CancelResponse {
  Reservation reservation = 1;
}

message GetRequest {
  string id = 1;
}

message GetResponse {
  Reservation reservation = 1;
}

message QueryRequest {
  string resource_id = 1;
  string user_id = 2;

  ReservationStatus status = 3;
  google.protobuf.Timestamp start = 4;
  google.protobuf.Timestamp end = 5;
}

service ReservationService {
  rpc reserve(ReserveRequest) return ( ReserveResponse );
  rpc confirm(ConfirmRequest) return  {ConfirmResponse};
  rpc update(UpdateRequest) return ( UpdateResponse );
  rpc cancel(CancelRequest) return ( CancelResponse );
  rpc get(GetRequest) return ( GetResponse );
  rpc query(QueryRequest) return ( stream Reservation );
}
```

For implementation-oriented RFCs (e.g. for compiler internals), this section should focus on how compiler contributors should think about the change, and give examples of its concrete impact. For policy RFCs, this section should provide an example-driven introduction to the policy, and explain its impact in concrete terms.

## Reference-level explanation

This is the technical portion of the RFC. Explain the design in sufficient detail that:

- Its interaction with other features is clear.
- It is reasonably clear how the feature would be implemented.
- Corner cases are dissected by example.

The section should return to the examples given in the previous section, and explain more fully how the detailed proposal makes those examples work.

## Drawbacks

Why should we *not* do this?

## Rationale and alternatives

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not choosing them?
- What is the impact of not doing this?

## Prior art

Discuss prior art, both the good and the bad, in relation to this proposal.
A few examples of what this can include are:

- For language, library, cargo, tools, and compiler proposals: Does this feature exist in other programming languages and what experience have their community had?
- For community proposals: Is this done by some other community and what were their experiences with it?
- For other teams: What lessons can we learn from what other communities have done here?
- Papers: Are there any published papers or great posts that discuss this? If you have some relevant papers to refer to, this can serve as a more detailed theoretical background.

This section is intended to encourage you as an author to think about the lessons from other languages, provide readers of your RFC with a fuller picture.
If there is no prior art, that is fine - your ideas are interesting to us whether they are brand new or if it is an adaptation from other languages.

Note that while precedent set by other languages is some motivation, it does not on its own motivate an RFC.
Please also take into consideration that rust sometimes intentionally diverges from common language features.

## Unresolved questions

- What parts of the design do you expect to resolve through the RFC process before this gets merged?
- What parts of the design do you expect to resolve through the implementation of this feature before stabilization?
- What related issues do you consider out of scope for this RFC that could be addressed in the future independently of the solution that comes out of this RFC?

## Future possibilities

Think about what the natural extension and evolution of your proposal would
be and how it would affect the language and project as a whole in a holistic
way. Try to use this section as a tool to more fully consider all possible
interactions with the project and language in your proposal.
Also consider how this all fits into the roadmap for the project
and of the relevant sub-team.

This is also a good place to "dump ideas", if they are out of scope for the
RFC you are writing but otherwise related.

If you have tried and cannot think of any future possibilities,
you may simply state that you cannot think of anything.

Note that having something written down in the future-possibilities section
is not a reason to accept the current or a future RFC; such notes should be
in the section on motivation or rationale in this or subsequent RFCs.
The section merely provides additional information.
