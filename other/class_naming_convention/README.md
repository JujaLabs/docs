# JuJa Platform

## Class Naming Convention (DRAFT VERSION)

### Main Classes

* Classes realizing Dependency Inversion pattern should start with the name of their implemented interface ending with "Impl" suffix (e.g. KeeperService → KeeperServiceImpl)

### Test Classes

#### Unit Tests
* Test classes should be named after classes they test with the "Test" suffix added (e.g. KeepersSlackCommandController → KeepersSlackCommandControllerTest).
* Your test should reflect the name of the method begin tested, not only the requirements from it. Recommended pattern: [MethodName_StateUnderTest_ExpectedBehavior] (e.g. getKeepers_ExceptionThrown_IsKeepersMicroserviceExchangeException).
* Variable names should express the expected input and state, the name does have to represent the intent of the test author (e.g. String TOKEN_WRONG = "wrongSlackToken").