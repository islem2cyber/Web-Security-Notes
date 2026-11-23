# Programming Knowledge Required for File Upload Security

This section summarizes the PHP and JavaScript concepts needed to review upload code. It is not a complete language course.

## PHP Fundamentals Relevant to File Upload

### Variables and arrays

PHP variables start with `$`. Arrays are ordered maps and associative arrays use named keys.

```php
$allowedTypes = ['image/png', 'image/jpeg'];
$metadata = [
    'field' => 'avatar',
    'sizeLimit' => 2_000_000,
];
```

### `$_GET`, `$_POST`, `$_FILES`, and `$_SERVER`

- `$_GET` contains URL query parameters.
- `$_POST` contains submitted form fields for supported request encodings.
- `$_FILES` contains information about successfully received multipart uploads.
- `$_SERVER` contains server and request metadata.

Typical `$_FILES` metadata can include `name`, `type`, `tmp_name`, `error`, and `size`.

```php
if (!isset($_FILES['avatar']) || !is_uploaded_file($_FILES['avatar']['tmp_name'])) {
    http_response_code(400);
    exit;
}

$upload = $_FILES['avatar'];
$originalName = basename($upload['name']);
$clientMime = $upload['type'];
$temporaryPath = $upload['tmp_name'];
$size = $upload['size'];
$error = $upload['error'];
```

The client-controlled `name` and `type` values are not trusted. `is_uploaded_file()` verifies that PHP received the path as an upload, but it does not validate format or size.

### `move_uploaded_file()`

`move_uploaded_file()` moves a valid PHP upload to a destination. The destination must be server-selected and the move should be checked.

### `pathinfo()` and `basename()`

- `pathinfo()` extracts path information such as directory or extension.
- `basename()` removes directory components.

Do not use the original filename as a destination. Both functions are useful for display or metadata, but neither creates a security boundary.

### `finfo_file()`

The Fileinfo API can inspect detected content and return a MIME type. It is one validation signal.

```php
$finfo = new finfo(FILEINFO_MIME_TYPE);
$detectedMime = $finfo->file($temporaryPath);
```

The result should be compared with an allowlist and followed by format-specific validation. It does not prove safety against polyglot or embedded active content.

### Error handling

Check `UPLOAD_ERR_OK`, size, destination, and filesystem failures. Use a generic client response and log safe server-side details.

### Safe PHP upload structure

```php
if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
    http_response_code(405);
    exit;
}

if (!isset($_FILES['avatar'])) {
    http_response_code(400);
    exit;
}

$upload = $_FILES['avatar'];

if ($upload['error'] !== UPLOAD_ERR_OK) {
    http_response_code(400);
    exit;
}

if ($upload['size'] > 2_000_000) {
    http_response_code(413);
    exit;
}

$tempPath = $upload['tmp_name'];
if (!is_uploaded_file($tempPath)) {
    http_response_code(400);
    exit;
}

$detectedType = (new finfo(FILEINFO_MIME_TYPE))->file($tempPath);
if (!in_array($detectedType, ['image/png', 'image/jpeg'], true)) {
    http_response_code(415);
    exit;
}

$base = __DIR__ . DIRECTORY_SEPARATOR . 'private_uploads';
$name = bin2hex(random_bytes(16));
$destination = $base . DIRECTORY_SEPARATOR . $name;

if (!is_dir($base) && !mkdir($base, 0700, true) && !is_dir($base)) {
    http_response_code(500);
    exit;
}

if (!move_uploaded_file($tempPath, $destination)) {
    http_response_code(500);
    exit;
}

chmod($destination, 0600);
echo 'Upload stored';
```

This is a safe structural example, not a complete image parser or antivirus workflow. Additional format-specific parsing and file re-encoding may be required.

## JavaScript Fundamentals Relevant to File Upload

### File input and the File API

An HTML file input represents browser-selected files:

```html
<input id="avatar" type="file" accept="image/png,image/jpeg">
```

The `accept` attribute is advisory. It filters browser UI choices and does not validate uploaded bytes.

The browser API exposes a `FileList` and `File` objects. `File` includes metadata and a bytes stream.

```js
const input = document.querySelector("#avatar");
const files = input.files; // FileList
const file = files.item(0); // File or null
```

### `FormData`

`FormData` represents fields in a multipart form.

```js
const form = document.querySelector("#upload-form");

form.addEventListener("submit", async (event) => {
  event.preventDefault();

  const formData = new FormData(form);
  const response = await fetch("/upload", {
    method: "POST",
    body: formData,
  });

  if (!response.ok) {
    throw new Error("Upload failed");
  }
});
```

### Fetch API

The Fetch API sends HTTP requests. Do not set a manual `Content-Type: multipart/form-data` boundary when passing `FormData`; the browser adds it.

### Client validation

JavaScript can check size or name as a usability feature. It cannot replace server validation because browser code and `File` metadata are user-controlled.
