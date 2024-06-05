# Image Selection Component

This component allows users to select and preview an image before uploading it. It also provides functionality to remove or delete the selected image.

## Props

- **photoPath** (`String`): The URL of the current photo to be displayed. Default is an empty string.
- **photoAlt** (`String`): Alternative text for the current photo. Default is an empty string.
- **label** (`String`): The label for the photo input field. Default is 'Photo'.
- **deletePhotoRoute** (`String`): The route to be called when deleting the photo. Default is 'photo.destroy'.
- **isEnableRemove** (`Boolean`): Whether the remove photo button should be enabled. Default is `false`.
- **isEnableDelete** (`Boolean`): Whether the delete photo button should be enabled. Default is `false`.

## Events

- **update:modelValue**: Emitted when a new photo is selected.

## Methods

- **updatePhotoPreview**: Updates the photo preview when a new photo is selected.
- **selectNewPhoto**: Triggers the file input to select a new photo.
- **deletePhoto**: Deletes the current photo by calling the specified route.
- **clearPhotoFileInput**: Clears the file input field.
- **removeFile**: Removes the selected file and clears the photo preview.
