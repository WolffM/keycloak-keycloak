## Steps to reproduce
1. From the repository root, run `mvn -pl common -Dtest=ProfileTest#enablePreviewEnablesDynamicScopes test -DskipITs -DskipExamples`.
2. This test configures `keycloak.profile=preview` and checks whether `Profile.Feature.DYNAMIC_SCOPES` is enabled.
3. Before the fix, dynamic scopes were still classified as `Type.EXPERIMENTAL`, so preview mode did not enable them.

## Observed
The targeted test failed with `AssertionFailedError: expected: <true> but was: <false>` at `ProfileTest.enablePreviewEnablesDynamicScopes`. The surefire output showed `BUILD FAILURE` and the assertion stack trace, demonstrating that preview profile did not activate dynamic scopes. This confirms the feature had not yet been promoted from experimental to preview.

## Expected
Dynamic client scopes should be a preview feature. When Keycloak runs with preview profile enabled, `Profile.Feature.DYNAMIC_SCOPES` should be included among preview capabilities and be enabled accordingly. The focused profile test should pass, and the feature classification should reflect preview support instead of experimental status.
