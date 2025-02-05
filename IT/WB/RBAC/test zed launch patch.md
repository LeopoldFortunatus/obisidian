```
Index: test/integration/native_v2_test.go
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/test/integration/native_v2_test.go b/test/integration/native_v2_test.go
--- a/test/integration/native_v2_test.go	(revision 2965cf9c1dcaee100a9b5b5e414447b589117b1e)
+++ b/test/integration/native_v2_test.go	(date 1738048621265)
@@ -103,11 +103,4 @@
 		err := getVmControllerDBV1(cl.vmControllerV1Client)
 		require.NoError(t, err)
 	})
-
-	// TODO: keeep for debug purpose, remove before MR
-	t.Run("check relations with zed", func(t *testing.T) {
-		out, err := runZedRelationshipRead("namespace")
-		require.NoError(t, err)
-		t.Logf("namespace relations: %s", out)
-	})
 }
Index: test/integration/sandbox_test.go
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/test/integration/sandbox_test.go b/test/integration/sandbox_test.go
--- a/test/integration/sandbox_test.go	(revision 2965cf9c1dcaee100a9b5b5e414447b589117b1e)
+++ b/test/integration/sandbox_test.go	(date 1738048617019)
@@ -209,13 +209,6 @@
 		err := getVmControllerDBV1(cl.vmControllerV1Client)
 		require.NoError(t, err)
 	})
-
-	// TODO: keeep for debug purpose, remove before MR
-	t.Run("check relations with zed", func(t *testing.T) {
-		out, err := runZedRelationshipRead("namespace")
-		require.NoError(t, err)
-		t.Logf("namespace relations: %s", out)
-	})
 }
 
 // Run create vm scenario multiple times
Index: test/integration/helpers_test.go
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/test/integration/helpers_test.go b/test/integration/helpers_test.go
--- a/test/integration/helpers_test.go	(revision 2965cf9c1dcaee100a9b5b5e414447b589117b1e)
+++ b/test/integration/helpers_test.go	(date 1738079485506)
@@ -8,7 +8,6 @@
 	"fmt"
 	"log/slog"
 	"os"
-	"os/exec"
 	"testing"
 	"time"
 
@@ -45,8 +44,6 @@
 
 var globalClients *clients
 
-var testSpiceDBAddress string
-
 type clients struct {
 	spiceDBClient           *spicedbclient.Client
 	spiceDBClientAdmin      *spicedbadminclient.Client
@@ -161,7 +158,6 @@
 	addressVmController := fmt.Sprintf("%s:%d", testhelpers.IP, portVmController)
 	addressSpiceDB := fmt.Sprintf("%s:%d", testhelpers.IP, portSpiceDB)
 	addressSpiceDBAdmin := fmt.Sprintf("%s:%d", testhelpers.IP, portSpiceDBAdmin)
-	testSpiceDBAddress = addressSpiceDBAdmin
 
 	keysloaderStubsConfig := builders.NewKeysloaderConfig(
 		tokenExchangeAddress,
@@ -524,14 +520,3 @@
 		user4:     "user4-other-" + randomSuffix,
 	}
 }
-
-func runZedRelationshipRead(relation string) (string, error) {
-	runCmd := exec.Command("bash", "-c",
-		fmt.Sprintf("zed relationship --insecure --endpoint %s read %s",
-			testSpiceDBAddress, relation))
-	output, err := runCmd.CombinedOutput()
-	if err != nil {
-		return "", fmt.Errorf("failed to execute zed command: %w", err)
-	}
-	return string(output), nil
-}

```

```
func runZedRelationshipRead(relation string) (string, error) {
	runCmd := exec.Command("bash", "-c",
		fmt.Sprintf("zed relationship --insecure --endpoint %s read %s",
			testSpiceDBAddress, relation))
	output, err := runCmd.CombinedOutput()
	if err != nil {
		return "", fmt.Errorf("failed to execute zed command: %w", err)
	}
	return string(output), nil
}
```

```
	// TODO: keeep for debug purpose, remove before MR
	t.Run("check relations with zed", func(t *testing.T) {
		out, err := runZedRelationshipRead("namespace")
		require.NoError(t, err)
		t.Logf("namespace relations: %s", out)
	})
```