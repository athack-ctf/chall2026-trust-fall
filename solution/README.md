# How to Solve the Challenge?

the application have two main functionalities and two fields.
<img width="1080" height="2400" alt="Screenshot_1770700623" src="https://github.com/user-attachments/assets/3925b8c3-5f27-4ffd-b5ee-be0748d50132" />

the application will show us a message by clicking on "Install Official Compliance Pack":
<img width="1080" height="2400" alt="Screenshot_1770700769" src="https://github.com/user-attachments/assets/2fd02d17-3ab3-4bc8-9aee-4240fa9fa557" />

and it shows another message by clicking on withdeaw:
<img width="1080" height="2400" alt="Screenshot_1770700837" src="https://github.com/user-attachments/assets/ac05a35b-feae-402f-b4ef-70b75fb124c0" />

So what we see, is a crypto wallet application. but why it shows "DENY" and what is the compliance pack?
let's decompile the application using jadx:
<img width="1701" height="1272" alt="image" src="https://github.com/user-attachments/assets/a363a258-033f-4888-b83f-bc8defef7e67" />

MainActivity is our entrypoint.
MainActivity:

<img width="1348" height="612" alt="image" src="https://github.com/user-attachments/assets/8c807fa0-cf90-4b72-98c6-91c479f0664f" />

```Java
package com.athack.trustfall;

import android.content.res.Resources;
import android.os.Bundle;
import android.text.Editable;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import androidx.constraintlayout.widget.ConstraintLayout;
import com.athack.trustfall.PolicyEngine;
import kotlin.Metadata;
import kotlin.jvm.internal.Intrinsics;
import kotlin.text.StringsKt;

/* compiled from: MainActivity.kt */
@Metadata(m143d1 = {"\u0000\u0018\n\u0002\u0018\u0002\n\u0002\u0018\u0002\n\u0002\b\u0002\n\u0002\u0010\u0002\n\u0000\n\u0002\u0018\u0002\n\u0000\u0018\u00002\u00020\u0001B\u0005¢\u0006\u0002\u0010\u0002J\u0012\u0010\u0003\u001a\u00020\u00042\b\u0010\u0005\u001a\u0004\u0018\u00010\u0006H\u0014¨\u0006\u0007"}, m144d2 = {"Lcom/athack/trustfall/MainActivity;", "Landroidx/appcompat/app/AppCompatActivity;", "()V", "onCreate", "", "savedInstanceState", "Landroid/os/Bundle;", "app_release"}, m145k = 1, m146mv = {1, 9, 0}, m148xi = ConstraintLayout.LayoutParams.Table.LAYOUT_CONSTRAINT_VERTICAL_CHAINSTYLE)
/* loaded from: classes.dex */
public final class MainActivity extends AppCompatActivity {
    @Override // androidx.fragment.app.FragmentActivity, androidx.activity.ComponentActivity, androidx.core.app.ComponentActivity, android.app.Activity
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(C0537R.layout.activity_main);
        Button button = (Button) findViewById(C0537R.id.btnInstall);
        final TextView textView = (TextView) findViewById(C0537R.id.txtInstallStatus);
        final EditText editText = (EditText) findViewById(C0537R.id.edtAddress);
        final EditText editText2 = (EditText) findViewById(C0537R.id.edtSats);
        Button button2 = (Button) findViewById(C0537R.id.btnWithdraw);
        final TextView textView2 = (TextView) findViewById(C0537R.id.txtResult);
        editText.setText(PolicyEngine.Target.address);
        editText2.setText("1337");
        button.setOnClickListener(new View.OnClickListener() { // from class: com.athack.trustfall.MainActivity$$ExternalSyntheticLambda0
            @Override // android.view.View.OnClickListener
            public final void onClick(View view) {
                MainActivity.onCreate$lambda$0(this.f$0, textView, view);
            }
        });
        button2.setOnClickListener(new View.OnClickListener() { // from class: com.athack.trustfall.MainActivity$$ExternalSyntheticLambda1
            @Override // android.view.View.OnClickListener
            public final void onClick(View view) throws Resources.NotFoundException {
                MainActivity.onCreate$lambda$1(editText, editText2, this, textView2, view);
            }
        });
    }

    /* JADX INFO: Access modifiers changed from: private */
    public static final void onCreate$lambda$0(MainActivity this$0, TextView textView, View view) {
        String error;
        StringBuilder sb;
        Intrinsics.checkNotNullParameter(this$0, "this$0");
        InstallResult installResultInstallOfficialPack = PackInstaller.INSTANCE.installOfficialPack(this$0);
        if (installResultInstallOfficialPack.getOk()) {
            error = installResultInstallOfficialPack.getPath();
            sb = new StringBuilder("Installed: ");
        } else {
            error = installResultInstallOfficialPack.getError();
            sb = new StringBuilder("Install failed: ");
        }
        textView.setText(sb.append(error).toString());
    }

    /* JADX INFO: Access modifiers changed from: private */
    public static final void onCreate$lambda$1(EditText editText, EditText editText2, MainActivity this$0, TextView textView, View view) throws Resources.NotFoundException {
        String string;
        String string2;
        Long longOrNull;
        String string3;
        Intrinsics.checkNotNullParameter(this$0, "this$0");
        Editable text = editText.getText();
        String string4 = (text == null || (string3 = text.toString()) == null) ? null : StringsKt.trim((CharSequence) string3).toString();
        if (string4 == null) {
            string4 = "";
        }
        String str = string4;
        Editable text2 = editText2.getText();
        long jLongValue = (text2 == null || (string = text2.toString()) == null || (string2 = StringsKt.trim((CharSequence) string).toString()) == null || (longOrNull = StringsKt.toLongOrNull(string2)) == null) ? 0L : longOrNull.longValue();
        MainActivity mainActivity = this$0;
        PolicyLoadResult policyLoadResultLoadFromActivePack = PolicyEngine.INSTANCE.loadFromActivePack(mainActivity);
        if (!policyLoadResultLoadFromActivePack.getOk()) {
            textView.setText("Pack not ready: " + policyLoadResultLoadFromActivePack.getError());
            return;
        }
        PolicyEngine policyEngine = PolicyEngine.INSTANCE;
        PolicyPack policy = policyLoadResultLoadFromActivePack.getPolicy();
        Intrinsics.checkNotNull(policy);
        PolicyDecision policyDecisionEvaluateWithdrawal = policyEngine.evaluateWithdrawal(policy, PolicyEngine.Target.userId, jLongValue, str);
        if (policyDecisionEvaluateWithdrawal != PolicyDecision.ALLOW) {
            textView.setText("Decision: " + policyDecisionEvaluateWithdrawal.name());
            return;
        }
        String strPolicyToken = PolicyEngine.INSTANCE.policyToken(policyLoadResultLoadFromActivePack.getPolicy(), PolicyEngine.Target.userId, jLongValue, str);
        String strTryDecryptFlag = CryptoVault.INSTANCE.tryDecryptFlag(mainActivity, strPolicyToken, PolicyEngine.Target.userId, jLongValue, str);
        textView.setText((strTryDecryptFlag != null ? new StringBuilder("Decision: ALLOW\nToken: ").append(strPolicyToken).append("\nFLAG: ").append(strTryDecryptFlag) : new StringBuilder("Decision: ALLOW\nToken: ").append(strPolicyToken).append("\nNo flag")).toString());
    }
}
```
in this code we can see an event handler for a botton and its calling onCreate$lambda$0:
```Java
        button.setOnClickListener(new View.OnClickListener() { // from class: com.athack.trustfall.MainActivity$$ExternalSyntheticLambda0
            @Override // android.view.View.OnClickListener
            public final void onClick(View view) {
                MainActivity.onCreate$lambda$0(this.f$0, textView, view);
            }
        });
```

onCreate$lambda$0:
```Java
public static final void onCreate$lambda$0(MainActivity this$0, TextView textView, View view) {
        String error;
        StringBuilder sb;
        Intrinsics.checkNotNullParameter(this$0, "this$0");
        InstallResult installResultInstallOfficialPack = PackInstaller.INSTANCE.installOfficialPack(this$0);
        if (installResultInstallOfficialPack.getOk()) {
            error = installResultInstallOfficialPack.getPath();
            sb = new StringBuilder("Installed: ");
        } else {
            error = installResultInstallOfficialPack.getError();
            sb = new StringBuilder("Install failed: ");
        }
        textView.setText(sb.append(error).toString());
    }
```
as we can see the onCreate$lambda$0 event is calling PackInstaller.INSTANCE.installOfficialPack. we can guess that its the functionality of "Install Official Compliance Pack" botton. for seeing what PackInstaller is doing we have to take a look at it's class:
<img width="1198" height="621" alt="image" src="https://github.com/user-attachments/assets/52aa65ad-9a7f-4151-8112-6fdf62db4437" />

```Java
package com.athack.trustfall;

import android.content.Context;
import android.content.res.Resources;
import android.util.Base64;
import androidx.constraintlayout.widget.ConstraintLayout;
import java.io.File;
import java.io.FileOutputStream;
import java.io.InputStream;
import java.security.InvalidKeyException;
import java.security.KeyFactory;
import java.security.NoSuchAlgorithmException;
import java.security.PublicKey;
import java.security.Signature;
import java.security.SignatureException;
import java.security.spec.InvalidKeySpecException;
import java.security.spec.X509EncodedKeySpec;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;
import kotlin.Metadata;
import kotlin.Unit;
import kotlin.jvm.internal.Intrinsics;
import kotlin.p001io.ByteStreamsKt;
import kotlin.p001io.CloseableKt;
import kotlin.p001io.FilesKt;
import kotlin.text.StringsKt;

/* compiled from: PackInstaller.kt */
@Metadata(m143d1 = {"\u0000,\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0002\b\u0002\n\u0002\u0010\u000e\n\u0000\n\u0002\u0018\u0002\n\u0000\n\u0002\u0018\u0002\n\u0000\n\u0002\u0010\u000b\n\u0000\n\u0002\u0010\u0012\n\u0002\b\u0002\bÆ\u0002\u0018\u00002\u00020\u0001B\u0007\b\u0002¢\u0006\u0002\u0010\u0002J\u000e\u0010\u0005\u001a\u00020\u00062\u0006\u0010\u0007\u001a\u00020\bJ \u0010\t\u001a\u00020\n2\u0006\u0010\u0007\u001a\u00020\b2\u0006\u0010\u000b\u001a\u00020\f2\u0006\u0010\r\u001a\u00020\u0004H\u0002R\u000e\u0010\u0003\u001a\u00020\u0004X\u0082T¢\u0006\u0002\n\u0000¨\u0006\u000e"}, m144d2 = {"Lcom/athack/trustfall/PackInstaller;", "", "()V", "AssetName", "", "installOfficialPack", "Lcom/athack/trustfall/InstallResult;", "ctx", "Landroid/content/Context;", "verifyPluginSignature", "", "pluginJson", "", "sigB64", "app_release"}, m145k = 1, m146mv = {1, 9, 0}, m148xi = ConstraintLayout.LayoutParams.Table.LAYOUT_CONSTRAINT_VERTICAL_CHAINSTYLE)
/* loaded from: classes.dex */
public final class PackInstaller {
    private static final String AssetName = "official_pack.zip";
    public static final PackInstaller INSTANCE = new PackInstaller();

    private PackInstaller() {
    }

    public final InstallResult installOfficialPack(Context ctx) {
        Intrinsics.checkNotNullParameter(ctx, "ctx");
        File externalFilesDir = ctx.getExternalFilesDir(null);
        if (externalFilesDir == null) {
            return new InstallResult(false, null, "no external files dir", 2, null);
        }
        File file = new File(externalFilesDir, "packs/active");
        if (file.exists()) {
            FilesKt.deleteRecursively(file);
        }
        if (!file.mkdirs()) {
            return new InstallResult(false, null, "cannot create active dir", 2, null);
        }
        try {
            FileOutputStream fileOutputStreamOpen = ctx.getAssets().open(AssetName);
            try {
                fileOutputStreamOpen = new ZipInputStream(fileOutputStreamOpen);
                try {
                    ZipInputStream zipInputStream = fileOutputStreamOpen;
                    while (true) {
                        ZipEntry nextEntry = zipInputStream.getNextEntry();
                        if (nextEntry == null) {
                            break;
                        }
                        Intrinsics.checkNotNull(nextEntry);
                        if (nextEntry.isDirectory()) {
                            new File(file, nextEntry.getName()).mkdirs();
                        } else {
                            File file2 = new File(file, nextEntry.getName());
                            File parentFile = file2.getParentFile();
                            if (parentFile != null) {
                                parentFile.mkdirs();
                            }
                            fileOutputStreamOpen = new FileOutputStream(file2);
                            try {
                                ByteStreamsKt.copyTo$default(zipInputStream, fileOutputStreamOpen, 0, 2, null);
                                CloseableKt.closeFinally(fileOutputStreamOpen, null);
                            } finally {
                            }
                        }
                        zipInputStream.closeEntry();
                    }
                    Unit unit = Unit.INSTANCE;
                    CloseableKt.closeFinally(fileOutputStreamOpen, null);
                    Unit unit2 = Unit.INSTANCE;
                    CloseableKt.closeFinally(fileOutputStreamOpen, null);
                    File file3 = new File(file, "plugin/plugin.json");
                    File file4 = new File(file, "plugin/plugin.sig");
                    File file5 = new File(file, "policy/policy.bin");
                    if (file3.isFile() && file4.isFile()) {
                        if (!file5.isFile()) {
                            return new InstallResult(false, null, "missing policy", 2, null);
                        }
                        if (!verifyPluginSignature(ctx, FilesKt.readBytes(file3), StringsKt.trim((CharSequence) FilesKt.readText$default(file4, null, 1, null)).toString())) {
                            return new InstallResult(false, null, "bad signature", 2, null);
                        }
                        String absolutePath = file.getAbsolutePath();
                        Intrinsics.checkNotNullExpressionValue(absolutePath, "getAbsolutePath(...)");
                        return new InstallResult(true, absolutePath, null, 4, null);
                    }
                    return new InstallResult(false, null, "missing plugin artifacts", 2, null);
                } finally {
                }
            } finally {
            }
        } catch (Throwable th) {
            boolean z = false;
            String str = null;
            String simpleName = th.getClass().getSimpleName();
            String message = th.getMessage();
            if (message == null) {
                message = "";
            }
            return new InstallResult(z, str, simpleName + ":" + message, 2, null);
        }
    }

    private final boolean verifyPluginSignature(Context ctx, byte[] pluginJson, String sigB64) throws InvalidKeySpecException, Resources.NotFoundException, NoSuchAlgorithmException, SignatureException, InvalidKeyException {
        InputStream inputStreamOpenRawResource = ctx.getResources().openRawResource(C0537R.raw.pack_pub);
        try {
            InputStream inputStream = inputStreamOpenRawResource;
            Intrinsics.checkNotNull(inputStream);
            byte[] bytes = ByteStreamsKt.readBytes(inputStream);
            CloseableKt.closeFinally(inputStreamOpenRawResource, null);
            PublicKey publicKeyGeneratePublic = KeyFactory.getInstance("RSA").generatePublic(new X509EncodedKeySpec(bytes));
            Signature signature = Signature.getInstance("SHA256withRSA");
            signature.initVerify(publicKeyGeneratePublic);
            signature.update(pluginJson);
            return signature.verify(Base64.decode(sigB64, 0));
        } finally {
        }
    }
}
```

The PackInstaller class is loading an asset called "official_pack.zip" and extract it into an external files directory. and also, there is a verifyPluginSignature functionality which garantie the integrity of files. but there is a catch. it only verifies the integrity of file3 and file4 where are "plugin/plugin.json" and "plugin/plugin.sig" and not checking the integrity of "policy/policy.bin":
```Java
if (file3.isFile() && file4.isFile()) {
                        if (!file5.isFile()) {
                            return new InstallResult(false, null, "missing policy", 2, null);
                        }
                        if (!verifyPluginSignature(ctx, FilesKt.readBytes(file3), StringsKt.trim((CharSequence) FilesKt.readText$default(file4, null, 1, null)).toString())) {
                            return new InstallResult(false, null, "bad signature", 2, null);
                        }
                        String absolutePath = file.getAbsolutePath();
                        Intrinsics.checkNotNullExpressionValue(absolutePath, "getAbsolutePath(...)");
                        return new InstallResult(true, absolutePath, null, 4, null);
                    }
```

For now, we don't know what is policy.bin and what application is doing with it. but we're gonna find out soon. let's move on for now. after verifying the file's integrity it'll check the installation result with InstallResult class:
```Java
                    return new InstallResult(false, null, "missing plugin artifacts", 2, null);
```

InstallResult:
<img width="1284" height="689" alt="image" src="https://github.com/user-attachments/assets/fe67587d-16ba-4ed9-8a1a-20b5a21021d8" />

```Java
package com.athack.trustfall;

import androidx.constraintlayout.widget.ConstraintLayout;
import kotlin.Metadata;
import kotlin.jvm.internal.DefaultConstructorMarker;
import kotlin.jvm.internal.Intrinsics;

/* compiled from: PackInstaller.kt */
@Metadata(m143d1 = {"\u0000 \n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0000\n\u0002\u0010\u000b\n\u0000\n\u0002\u0010\u000e\n\u0002\b\u000e\n\u0002\u0010\b\n\u0002\b\u0002\b\u0086\b\u0018\u00002\u00020\u0001B!\u0012\u0006\u0010\u0002\u001a\u00020\u0003\u0012\b\b\u0002\u0010\u0004\u001a\u00020\u0005\u0012\b\b\u0002\u0010\u0006\u001a\u00020\u0005¢\u0006\u0002\u0010\u0007J\t\u0010\r\u001a\u00020\u0003HÆ\u0003J\t\u0010\u000e\u001a\u00020\u0005HÆ\u0003J\t\u0010\u000f\u001a\u00020\u0005HÆ\u0003J'\u0010\u0010\u001a\u00020\u00002\b\b\u0002\u0010\u0002\u001a\u00020\u00032\b\b\u0002\u0010\u0004\u001a\u00020\u00052\b\b\u0002\u0010\u0006\u001a\u00020\u0005HÆ\u0001J\u0013\u0010\u0011\u001a\u00020\u00032\b\u0010\u0012\u001a\u0004\u0018\u00010\u0001HÖ\u0003J\t\u0010\u0013\u001a\u00020\u0014HÖ\u0001J\t\u0010\u0015\u001a\u00020\u0005HÖ\u0001R\u0011\u0010\u0006\u001a\u00020\u0005¢\u0006\b\n\u0000\u001a\u0004\b\b\u0010\tR\u0011\u0010\u0002\u001a\u00020\u0003¢\u0006\b\n\u0000\u001a\u0004\b\n\u0010\u000bR\u0011\u0010\u0004\u001a\u00020\u0005¢\u0006\b\n\u0000\u001a\u0004\b\f\u0010\t¨\u0006\u0016"}, m144d2 = {"Lcom/athack/trustfall/InstallResult;", "", "ok", "", "path", "", "error", "(ZLjava/lang/String;Ljava/lang/String;)V", "getError", "()Ljava/lang/String;", "getOk", "()Z", "getPath", "component1", "component2", "component3", "copy", "equals", "other", "hashCode", "", "toString", "app_release"}, m145k = 1, m146mv = {1, 9, 0}, m148xi = ConstraintLayout.LayoutParams.Table.LAYOUT_CONSTRAINT_VERTICAL_CHAINSTYLE)
/* loaded from: classes.dex */
public final /* data */ class InstallResult {
    private final String error;
    private final boolean ok;
    private final String path;

    public static /* synthetic */ InstallResult copy$default(InstallResult installResult, boolean z, String str, String str2, int i, Object obj) {
        if ((i & 1) != 0) {
            z = installResult.ok;
        }
        if ((i & 2) != 0) {
            str = installResult.path;
        }
        if ((i & 4) != 0) {
            str2 = installResult.error;
        }
        return installResult.copy(z, str, str2);
    }

    /* renamed from: component1, reason: from getter */
    public final boolean getOk() {
        return this.ok;
    }

    /* renamed from: component2, reason: from getter */
    public final String getPath() {
        return this.path;
    }

    /* renamed from: component3, reason: from getter */
    public final String getError() {
        return this.error;
    }

    public final InstallResult copy(boolean ok, String path, String error) {
        Intrinsics.checkNotNullParameter(path, "path");
        Intrinsics.checkNotNullParameter(error, "error");
        return new InstallResult(ok, path, error);
    }

    public boolean equals(Object other) {
        if (this == other) {
            return true;
        }
        if (!(other instanceof InstallResult)) {
            return false;
        }
        InstallResult installResult = (InstallResult) other;
        return this.ok == installResult.ok && Intrinsics.areEqual(this.path, installResult.path) && Intrinsics.areEqual(this.error, installResult.error);
    }

    public final String getError() {
        return this.error;
    }

    public final boolean getOk() {
        return this.ok;
    }

    public final String getPath() {
        return this.path;
    }

    public int hashCode() {
        return (((Boolean.hashCode(this.ok) * 31) + this.path.hashCode()) * 31) + this.error.hashCode();
    }

    public String toString() {
        return "InstallResult(ok=" + this.ok + ", path=" + this.path + ", error=" + this.error + ")";
    }

    public InstallResult(boolean z, String path, String error) {
        Intrinsics.checkNotNullParameter(path, "path");
        Intrinsics.checkNotNullParameter(error, "error");
        this.ok = z;
        this.path = path;
        this.error = error;
    }

    public /* synthetic */ InstallResult(boolean z, String str, String str2, int i, DefaultConstructorMarker defaultConstructorMarker) {
        this(z, (i & 2) != 0 ? "" : str, (i & 4) != 0 ? "" : str2);
    }
}
```

So for now, we understood that what "Install Official Compliance Pack" botton is exactly doing. let's get back to MainActivity:
```Java
button2.setOnClickListener(new View.OnClickListener() { // from class: com.athack.trustfall.MainActivity$$ExternalSyntheticLambda1
            @Override // android.view.View.OnClickListener
            public final void onClick(View view) throws Resources.NotFoundException {
                MainActivity.onCreate$lambda$1(editText, editText2, this, textView2, view);
            }
        });
```

we can see another event handler for another botton which is calling onCreate$lambda$1:
```Java
public static final void onCreate$lambda$1(EditText editText, EditText editText2, MainActivity this$0, TextView textView, View view) throws Resources.NotFoundException {
        String string;
        String string2;
        Long longOrNull;
        String string3;
        Intrinsics.checkNotNullParameter(this$0, "this$0");
        Editable text = editText.getText();
        String string4 = (text == null || (string3 = text.toString()) == null) ? null : StringsKt.trim((CharSequence) string3).toString();
        if (string4 == null) {
            string4 = "";
        }
        String str = string4;
        Editable text2 = editText2.getText();
        long jLongValue = (text2 == null || (string = text2.toString()) == null || (string2 = StringsKt.trim((CharSequence) string).toString()) == null || (longOrNull = StringsKt.toLongOrNull(string2)) == null) ? 0L : longOrNull.longValue();
        MainActivity mainActivity = this$0;
        PolicyLoadResult policyLoadResultLoadFromActivePack = PolicyEngine.INSTANCE.loadFromActivePack(mainActivity);
        if (!policyLoadResultLoadFromActivePack.getOk()) {
            textView.setText("Pack not ready: " + policyLoadResultLoadFromActivePack.getError());
            return;
        }
        PolicyEngine policyEngine = PolicyEngine.INSTANCE;
        PolicyPack policy = policyLoadResultLoadFromActivePack.getPolicy();
        Intrinsics.checkNotNull(policy);
        PolicyDecision policyDecisionEvaluateWithdrawal = policyEngine.evaluateWithdrawal(policy, PolicyEngine.Target.userId, jLongValue, str);
        if (policyDecisionEvaluateWithdrawal != PolicyDecision.ALLOW) {
            textView.setText("Decision: " + policyDecisionEvaluateWithdrawal.name());
            return;
        }
        String strPolicyToken = PolicyEngine.INSTANCE.policyToken(policyLoadResultLoadFromActivePack.getPolicy(), PolicyEngine.Target.userId, jLongValue, str);
        String strTryDecryptFlag = CryptoVault.INSTANCE.tryDecryptFlag(mainActivity, strPolicyToken, PolicyEngine.Target.userId, jLongValue, str);
        textView.setText((strTryDecryptFlag != null ? new StringBuilder("Decision: ALLOW\nToken: ").append(strPolicyToken).append("\nFLAG: ").append(strTryDecryptFlag) : new StringBuilder("Decision: ALLOW\nToken: ").append(strPolicyToken).append("\nNo flag")).toString());
    }
```

we can see that this code is handling the withdraw process and calling PolicyEngine class for checking the transaction policy.
PolicyEngine:
<img width="1093" height="460" alt="image" src="https://github.com/user-attachments/assets/11f1b179-3cc3-482e-917d-59eb25457239" />

```Java
package com.athack.trustfall;

import android.content.Context;
import androidx.constraintlayout.widget.ConstraintLayout;
import java.io.File;
import java.nio.ByteBuffer;
import java.nio.ByteOrder;
import java.security.MessageDigest;
import java.util.Arrays;
import java.util.HashSet;
import kotlin.Metadata;
import kotlin.UShort;
import kotlin.collections.ArraysKt;
import kotlin.jvm.functions.Function1;
import kotlin.jvm.internal.Intrinsics;
import kotlin.p001io.FilesKt;
import kotlin.text.Charsets;

/* compiled from: PolicyEngine.kt */
@Metadata(m143d1 = {"\u0000>\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0002\b\u0002\n\u0002\u0010\u000e\n\u0000\n\u0002\u0010\u0012\n\u0002\b\u0002\n\u0002\u0010\t\n\u0002\b\u0002\n\u0002\u0018\u0002\n\u0000\n\u0002\u0018\u0002\n\u0002\b\u0004\n\u0002\u0018\u0002\n\u0000\n\u0002\u0018\u0002\n\u0002\b\u0006\bÆ\u0002\u0018\u00002\u00020\u0001:\u0001\u001aB\u0007\b\u0002¢\u0006\u0002\u0010\u0002J\u001e\u0010\u0005\u001a\u00020\u00062\u0006\u0010\u0007\u001a\u00020\u00042\u0006\u0010\b\u001a\u00020\t2\u0006\u0010\n\u001a\u00020\u0004J&\u0010\u000b\u001a\u00020\f2\u0006\u0010\r\u001a\u00020\u000e2\u0006\u0010\u0007\u001a\u00020\u00042\u0006\u0010\b\u001a\u00020\t2\u0006\u0010\n\u001a\u00020\u0004J\u000e\u0010\u000f\u001a\u00020\u00062\u0006\u0010\u0010\u001a\u00020\u0004J\u000e\u0010\u0011\u001a\u00020\u00062\u0006\u0010\u0010\u001a\u00020\u0004J\u000e\u0010\u0012\u001a\u00020\u00132\u0006\u0010\u0014\u001a\u00020\u0015J\u0010\u0010\u0016\u001a\u00020\u000e2\u0006\u0010\u0017\u001a\u00020\u0006H\u0002J&\u0010\u0018\u001a\u00020\u00042\u0006\u0010\r\u001a\u00020\u000e2\u0006\u0010\u0007\u001a\u00020\u00042\u0006\u0010\b\u001a\u00020\t2\u0006\u0010\n\u001a\u00020\u0004J\u0010\u0010\u0019\u001a\u00020\u00062\u0006\u0010\u0017\u001a\u00020\u0006H\u0002R\u000e\u0010\u0003\u001a\u00020\u0004X\u0082T¢\u0006\u0002\n\u0000¨\u0006\u001b"}, m144d2 = {"Lcom/athack/trustfall/PolicyEngine;", "", "()V", "Domain", "", "aad", "", "userId", "sats", "", "address", "evaluateWithdrawal", "Lcom/athack/trustfall/PolicyDecision;", "policy", "Lcom/athack/trustfall/PolicyPack;", "iv", "tokenHex", "key", "loadFromActivePack", "Lcom/athack/trustfall/PolicyLoadResult;", "ctx", "Landroid/content/Context;", "parsePolicy", "b", "policyToken", "sha256", "Target", "app_release"}, m145k = 1, m146mv = {1, 9, 0}, m148xi = ConstraintLayout.LayoutParams.Table.LAYOUT_CONSTRAINT_VERTICAL_CHAINSTYLE)
/* loaded from: classes.dex */
public final class PolicyEngine {
    private static final String Domain = "TRUSTFALL";
    public static final PolicyEngine INSTANCE = new PolicyEngine();

    /* compiled from: PolicyEngine.kt */
    @Metadata(m143d1 = {"\u0000\u001a\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0002\b\u0002\n\u0002\u0010\u000e\n\u0000\n\u0002\u0010\b\n\u0002\b\u0002\bÆ\u0002\u0018\u00002\u00020\u0001B\u0007\b\u0002¢\u0006\u0002\u0010\u0002R\u000e\u0010\u0003\u001a\u00020\u0004X\u0086T¢\u0006\u0002\n\u0000R\u000e\u0010\u0005\u001a\u00020\u0006X\u0086T¢\u0006\u0002\n\u0000R\u000e\u0010\u0007\u001a\u00020\u0004X\u0086T¢\u0006\u0002\n\u0000¨\u0006\b"}, m144d2 = {"Lcom/athack/trustfall/PolicyEngine$Target;", "", "()V", "address", "", "sats", "", "userId", "app_release"}, m145k = 1, m146mv = {1, 9, 0}, m148xi = ConstraintLayout.LayoutParams.Table.LAYOUT_CONSTRAINT_VERTICAL_CHAINSTYLE)
    public static final class Target {
        public static final Target INSTANCE = new Target();
        public static final String address = "bc1qctf0fficialwithdraw0000";
        public static final int sats = 1337;
        public static final String userId = "guest";

        private Target() {
        }
    }

    private PolicyEngine() {
    }

    public final PolicyLoadResult loadFromActivePack(Context ctx) {
        Intrinsics.checkNotNullParameter(ctx, "ctx");
        File externalFilesDir = ctx.getExternalFilesDir(null);
        if (externalFilesDir == null) {
            return new PolicyLoadResult(false, null, "no external files dir", 2, null);
        }
        File file = new File(externalFilesDir, "packs/active/policy/policy.bin");
        if (!file.isFile()) {
            return new PolicyLoadResult(false, null, "policy missing", 2, null);
        }
        try {
            return new PolicyLoadResult(true, parsePolicy(FilesKt.readBytes(file)), null, 4, null);
        } catch (Throwable th) {
            boolean z = false;
            PolicyPack policyPack = null;
            String simpleName = th.getClass().getSimpleName();
            String message = th.getMessage();
            if (message == null) {
                message = "";
            }
            return new PolicyLoadResult(z, policyPack, simpleName + ":" + message, 2, null);
        }
    }

    public final PolicyDecision evaluateWithdrawal(PolicyPack policy, String userId, long sats, String address) {
        Intrinsics.checkNotNullParameter(policy, "policy");
        Intrinsics.checkNotNullParameter(userId, "userId");
        Intrinsics.checkNotNullParameter(address, "address");
        if (policy.getDeny().contains(address)) {
            return PolicyDecision.DENY;
        }
        if (sats >= 1000000) {
            return PolicyDecision.REVIEW;
        }
        return PolicyDecision.ALLOW;
    }

    public final String policyToken(PolicyPack policy, String userId, long sats, String address) {
        Intrinsics.checkNotNullParameter(policy, "policy");
        Intrinsics.checkNotNullParameter(userId, "userId");
        Intrinsics.checkNotNullParameter(address, "address");
        byte[] bytes = ("TRUSTFALL|" + userId + "|" + sats + "|" + address + "|" + policy.getSecret()).getBytes(Charsets.UTF_8);
        Intrinsics.checkNotNullExpressionValue(bytes, "getBytes(...)");
        return ArraysKt.joinToString$default(sha256(bytes), (CharSequence) "", (CharSequence) null, (CharSequence) null, 0, (CharSequence) null, (Function1) new Function1<Byte, CharSequence>() { // from class: com.athack.trustfall.PolicyEngine.policyToken.1
            public final CharSequence invoke(byte b) {
                String str = String.format("%02x", Arrays.copyOf(new Object[]{Byte.valueOf(b)}, 1));
                Intrinsics.checkNotNullExpressionValue(str, "format(...)");
                return str;
            }

            @Override // kotlin.jvm.functions.Function1
            public /* bridge */ /* synthetic */ CharSequence invoke(Byte b) {
                return invoke(b.byteValue());
            }
        }, 30, (Object) null);
    }

    public final byte[] aad(String userId, long sats, String address) {
        Intrinsics.checkNotNullParameter(userId, "userId");
        Intrinsics.checkNotNullParameter(address, "address");
        byte[] bytes = ("TRUSTFALL|" + userId + "|" + sats + "|" + address).getBytes(Charsets.UTF_8);
        Intrinsics.checkNotNullExpressionValue(bytes, "getBytes(...)");
        return bytes;
    }

    /* renamed from: iv */
    public final byte[] m50iv(String tokenHex) {
        Intrinsics.checkNotNullParameter(tokenHex, "tokenHex");
        byte[] bytes = ("iv|" + tokenHex).getBytes(Charsets.UTF_8);
        Intrinsics.checkNotNullExpressionValue(bytes, "getBytes(...)");
        return ArraysKt.copyOfRange(sha256(bytes), 0, 12);
    }

    public final byte[] key(String tokenHex) {
        Intrinsics.checkNotNullParameter(tokenHex, "tokenHex");
        byte[] bytes = tokenHex.getBytes(Charsets.UTF_8);
        Intrinsics.checkNotNullExpressionValue(bytes, "getBytes(...)");
        return ArraysKt.copyOfRange(sha256(bytes), 0, 16);
    }

    private final byte[] sha256(byte[] b) {
        byte[] bArrDigest = MessageDigest.getInstance("SHA-256").digest(b);
        Intrinsics.checkNotNullExpressionValue(bArrDigest, "digest(...)");
        return bArrDigest;
    }

    private final PolicyPack parsePolicy(byte[] b) {
        if (b.length < 10) {
            throw new IllegalArgumentException("short");
        }
        if (b[0] != 78 || b[1] != 86 || b[2] != 80 || b[3] != 75) {
            throw new IllegalArgumentException("bad magic");
        }
        ByteBuffer byteBufferOrder = ByteBuffer.wrap(b).order(ByteOrder.LITTLE_ENDIAN);
        byteBufferOrder.position(4);
        if ((byteBufferOrder.getShort() & UShort.MAX_VALUE) != 1) {
            throw new IllegalArgumentException("bad version");
        }
        int i = byteBufferOrder.getShort() & UShort.MAX_VALUE;
        if (i < 1 || i > 256) {
            throw new IllegalArgumentException("bad secret len");
        }
        if (byteBufferOrder.remaining() < i + 2) {
            throw new IllegalArgumentException("truncated");
        }
        byte[] bArr = new byte[i];
        byteBufferOrder.get(bArr);
        String str = new String(bArr, Charsets.UTF_8);
        int i2 = byteBufferOrder.getShort() & UShort.MAX_VALUE;
        HashSet hashSet = new HashSet();
        for (int i3 = 0; i3 < i2 && byteBufferOrder.remaining() >= 2; i3++) {
            int i4 = byteBufferOrder.getShort() & UShort.MAX_VALUE;
            if (i4 < 1 || i4 > 200 || byteBufferOrder.remaining() < i4) {
                break;
            }
            byte[] bArr2 = new byte[i4];
            byteBufferOrder.get(bArr2);
            hashSet.add(new String(bArr2, Charsets.UTF_8));
        }
        return new PolicyPack(str, hashSet);
    }
}
```

now we can see that the policy.bin file got involved:
```Java
        File file = new File(externalFilesDir, "packs/active/policy/policy.bin");
```

the code is parsing the file using ParsePolicy function:
```Java
private final PolicyPack parsePolicy(byte[] b) {
        if (b.length < 10) {
            throw new IllegalArgumentException("short");
        }
        if (b[0] != 78 || b[1] != 86 || b[2] != 80 || b[3] != 75) {
            throw new IllegalArgumentException("bad magic");
        }
        ByteBuffer byteBufferOrder = ByteBuffer.wrap(b).order(ByteOrder.LITTLE_ENDIAN);
        byteBufferOrder.position(4);
        if ((byteBufferOrder.getShort() & UShort.MAX_VALUE) != 1) {
            throw new IllegalArgumentException("bad version");
        }
        int i = byteBufferOrder.getShort() & UShort.MAX_VALUE;
        if (i < 1 || i > 256) {
            throw new IllegalArgumentException("bad secret len");
        }
        if (byteBufferOrder.remaining() < i + 2) {
            throw new IllegalArgumentException("truncated");
        }
        byte[] bArr = new byte[i];
        byteBufferOrder.get(bArr);
        String str = new String(bArr, Charsets.UTF_8);
        int i2 = byteBufferOrder.getShort() & UShort.MAX_VALUE;
        HashSet hashSet = new HashSet();
        for (int i3 = 0; i3 < i2 && byteBufferOrder.remaining() >= 2; i3++) {
            int i4 = byteBufferOrder.getShort() & UShort.MAX_VALUE;
            if (i4 < 1 || i4 > 200 || byteBufferOrder.remaining() < i4) {
                break;
            }
            byte[] bArr2 = new byte[i4];
            byteBufferOrder.get(bArr2);
            hashSet.add(new String(bArr2, Charsets.UTF_8));
        }
        return new PolicyPack(str, hashSet);
    }
```

let's take a loot at policy.bin and map it to our code:
<img width="841" height="127" alt="image" src="https://github.com/user-attachments/assets/345830fc-c884-4266-9418-c311a45494e6" />

The function parsePolicy(byte[] b) implements a small custom binary format that produces a PolicyPack(secret, denySet) object. The file is parsed sequentially using a little-endian ByteBuffer. The parsing order is:
1- header checks (length + magic)
The parser first rejects too-short inputs:
```Java
if (b.length < 10) {
    throw new IllegalArgumentException("short");
}
```

2- version (uint16 LE)
The first 4 bytes must match ASCII "NVPK":
```Java
if (b[0] != 78 || b[1] != 86 || b[2] != 80 || b[3] != 75) {
    throw new IllegalArgumentException("bad magic");
}
```
<img width="760" height="114" alt="image" src="https://github.com/user-attachments/assets/71c35008-c558-4667-85df-a83a2acfb0b2" />


3- secret length (uint16 LE)
The next 16-bit field is the secret length:
```
int i = byteBufferOrder.getShort() & UShort.MAX_VALUE;
if (i < 1 || i > 256) {
    throw new IllegalArgumentException("bad secret len");
}
if (byteBufferOrder.remaining() < i + 2) {
    throw new IllegalArgumentException("truncated");
}
```

4- secret bytes (UTF-8)
The parser then reads exactly secretLen bytes, and converts them to a UTF-8 string:
```Java
byte[] bArr = new byte[i];
byteBufferOrder.get(bArr);
String str = new String(bArr, Charsets.UTF_8);
```

5- deny entry count (uint16 LE)
Immediately after the secret, the parser reads a 16-bit “number of deny entries”:
```Java
int i2 = byteBufferOrder.getShort() & UShort.MAX_VALUE;
HashSet hashSet = new HashSet();
```

6- deny list entries: repeated [len:uint16 LE][bytes:len]
The deny list is then parsed in a loop:
```Java
for (int i3 = 0; i3 < i2 && byteBufferOrder.remaining() >= 2; i3++) {
    int i4 = byteBufferOrder.getShort() & UShort.MAX_VALUE;
    if (i4 < 1 || i4 > 200 || byteBufferOrder.remaining() < i4) {
        break;
    }
    byte[] bArr2 = new byte[i4];
    byteBufferOrder.get(bArr2);
    hashSet.add(new String(bArr2, Charsets.UTF_8));
}
```

after parsing all these information the event handler code will check the parsed data with PolicyDecision class:
```Java
        PolicyDecision policyDecisionEvaluateWithdrawal = policyEngine.evaluateWithdrawal(policy, PolicyEngine.Target.userId, jLongValue, str);
        if (policyDecisionEvaluateWithdrawal != PolicyDecision.ALLOW) {
            textView.setText("Decision: " + policyDecisionEvaluateWithdrawal.name());
            return;
        }
```

PolicyDecision:
```Java
package com.athack.trustfall;

import androidx.constraintlayout.widget.ConstraintLayout;
import kotlin.Metadata;
import kotlin.enums.EnumEntries;
import kotlin.enums.EnumEntriesKt;

/* JADX WARN: Failed to restore enum class, 'enum' modifier and super class removed */
/* JADX WARN: Unknown enum class pattern. Please report as an issue! */
/* compiled from: PolicyEngine.kt */
@Metadata(m143d1 = {"\u0000\f\n\u0002\u0018\u0002\n\u0002\u0010\u0010\n\u0002\b\u0005\b\u0086\u0081\u0002\u0018\u00002\b\u0012\u0004\u0012\u00020\u00000\u0001B\u0007\b\u0002¢\u0006\u0002\u0010\u0002j\u0002\b\u0003j\u0002\b\u0004j\u0002\b\u0005¨\u0006\u0006"}, m144d2 = {"Lcom/athack/trustfall/PolicyDecision;", "", "(Ljava/lang/String;I)V", "ALLOW", "DENY", "REVIEW", "app_release"}, m145k = 1, m146mv = {1, 9, 0}, m148xi = ConstraintLayout.LayoutParams.Table.LAYOUT_CONSTRAINT_VERTICAL_CHAINSTYLE)
/* loaded from: classes.dex */
public final class PolicyDecision {
    private static final /* synthetic */ EnumEntries $ENTRIES;
    private static final /* synthetic */ PolicyDecision[] $VALUES;
    public static final PolicyDecision ALLOW = new PolicyDecision("ALLOW", 0);
    public static final PolicyDecision DENY = new PolicyDecision("DENY", 1);
    public static final PolicyDecision REVIEW = new PolicyDecision("REVIEW", 2);

    private static final /* synthetic */ PolicyDecision[] $values() {
        return new PolicyDecision[]{ALLOW, DENY, REVIEW};
    }

    public static EnumEntries<PolicyDecision> getEntries() {
        return $ENTRIES;
    }

    public static PolicyDecision valueOf(String str) {
        return (PolicyDecision) Enum.valueOf(PolicyDecision.class, str);
    }

    public static PolicyDecision[] values() {
        return (PolicyDecision[]) $VALUES.clone();
    }

    static {
        PolicyDecision[] policyDecisionArr$values = $values();
        $VALUES = policyDecisionArr$values;
        $ENTRIES = EnumEntriesKt.enumEntries(policyDecisionArr$values);
    }

    private PolicyDecision(String str, int i) {
    }
}
```

we can see our deny list in this code and our event handler will make the decision by parsing policy.bin and checking the value of it with the deny list:
```Java
    public static final PolicyDecision ALLOW = new PolicyDecision("ALLOW", 0);
    public static final PolicyDecision DENY = new PolicyDecision("DENY", 1);
    public static final PolicyDecision REVIEW = new PolicyDecision("REVIEW", 2);
```

So where is the vulnerable spot? there are two vulnerable spots. first is that the policy.bin is not signed or protected. second is the lack of integrity verification of policy.bin in the code. so we can chain these two vulnerable spot to perform the supply chain attack on the application. 
let's launch the attack. by changing the denylist value in policy.bin (offset 0x22) which is currently set to 0x01 to 0x00 we can ALLOW the transaction:
```Bash
printf '\x00\x00' | dd of=policy.bin bs=1 seek=$((0x22)) count=2 conv=notrunc status=none
```
<img width="1199" height="326" alt="image" src="https://github.com/user-attachments/assets/767b40bb-cc95-4f5d-99e7-dd97651679c6" />

now let's try to withdraw the money one more time:
<img width="1080" height="2400" alt="Screenshot_1770705083" src="https://github.com/user-attachments/assets/0b1cc516-07ca-4425-a607-a4a866de970f" />
