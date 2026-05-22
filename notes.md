## Steps to reproduce
1. From the repository root, run `./mvnw -pl common -Dtest=ProfileTest test -DskipITs` to confirm baseline tests pass.
2. Inspect `/tmp/workspace/WolffM/keycloak-keycloak/common/src/main/java/org/keycloak/common/Profile.java` and locate the `DYNAMIC_SCOPES` feature entry in the `Profile.Feature` enum.
3. Inspect `/tmp/workspace/WolffM/keycloak-keycloak/js/apps/admin-ui/src/context/server-info/__tests__/mock.json` to verify how the feature is classified in mocked server info.

## Observed
The feature was classified as `Type.EXPERIMENTAL` in `Profile.Feature`, and the admin UI server-info mock listed `DYNAMIC_SCOPES` under `experimentalFeatures` instead of `previewFeatures`. This demonstrates that the implementation still treats dynamic client scopes as experimental behavior rather than preview behavior, which does not match the requested promotion status for parameterizable OAuth scopes.

## Expected
Dynamic client scopes should be treated as a preview feature. The `DYNAMIC_SCOPES` entry should be classified as preview in profile metadata, and any related expectations (including representative server-info test fixtures) should reflect that it belongs to preview features rather than experimental features.
