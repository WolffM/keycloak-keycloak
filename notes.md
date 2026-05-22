## Steps to reproduce
1. From repository root, run `./mvnw -pl common -Dtest=ProfileTest#dynamicScopesTypeShouldBePreview test`.
2. This executes a focused test that verifies whether `Profile.Feature.DYNAMIC_SCOPES` is classified as a preview feature.
3. Before the fix, inspect `common/src/main/java/org/keycloak/common/Profile.java` and observe `DYNAMIC_SCOPES` was declared with `Type.EXPERIMENTAL`.

## Observed
The targeted test failed with `AssertionFailedError: expected: <PREVIEW> but was: <EXPERIMENTAL>`. Maven reported `BUILD FAILURE` and pointed to `ProfileTest.dynamicScopesTypeShouldBePreview`. This demonstrates that dynamic client scopes were still treated as experimental in the profile metadata, which contradicted the promotion request.

## Expected
Dynamic client scopes should be officially promoted to preview, so `Profile.Feature.DYNAMIC_SCOPES.getType()` should return `Profile.Feature.Type.PREVIEW`. The reproduction test should pass, and the feature should be included in preview behavior rather than experimental behavior so users can rely on the new parameterizable scope support at preview maturity level.
