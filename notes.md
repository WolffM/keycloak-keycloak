## Steps to reproduce
1. Open the repository at `/home/runner/work/keycloak-keycloak/keycloak-keycloak`.
2. Inspect the feature catalog in `common/src/main/java/org/keycloak/common/Profile.java`.
3. Locate `DYNAMIC_SCOPES` in the `Profile.Feature` enum and verify its feature type.
4. Run `mvn -pl common -Dtest=ProfileTest test -DskipITs -DskipTests=false` to confirm baseline behavior around profile feature types.

## Observed
The `DYNAMIC_SCOPES` feature was marked as `Type.EXPERIMENTAL`, so it was not treated as an official preview feature. In preview profile mode, this meant dynamic scopes were not promoted as intended by the issue goal. Baseline tests passed, but they reflected the old classification and did not assert that dynamic scopes are enabled through the preview profile.

## Expected
`DYNAMIC_SCOPES` should be promoted to `Type.PREVIEW` so that preview profile behavior includes dynamic client scopes as officially supported preview functionality. Tests should verify this expectation by asserting that dynamic scopes are enabled when the `preview` profile is selected.
