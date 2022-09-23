# Titre de la compétence

> ❌ A travailler

> ✔️ Auto validation par l'étudiant

## 🎓 J'ai compris et je peux expliquer

- les différences et points communs entre du code react et du code react native ❌ / ✔️
- ce que devient et comment est interprêté le code javascript dans une application react native ❌ / ✔️
- les avantages et inconvénients de react native ❌ / ✔️
- la différence entre react native et expo ❌ / ✔️
- les principales briques qui composent react native (core components) ❌ / ✔️
- comment écrire du style en react native ❌ / ✔️
- comment est géré le layout en react native ❌ / ✔️

## 💻 J'utilise

### Un exemple personnel commenté ❌ / ✔️\*

Composant d'upload de video ReactNative:

```Typescript
import React, {useContext, useEffect, useState} from 'react';
import {View, StyleSheet} from 'react-native';
import ImagePicker from 'react-native-image-crop-picker';
import {
  LogLevel,
  MEDIA_PICKER_MODE,
  MEDIA_TYPE,
  RawMedia,
  VideoUpload,
} from '../../../common/src/model';
import {isLocalFile} from '../../../common/src/services/media.service';
import {Logger} from '../../services/logger.service';
import {
  getDownloadURL,
  getVideoRotation,
  SHARED_OPTIONS,
  VIDEO_OPTIONS,
} from '../../services/media.service';
import {StoresContext} from '../../stores';
import {ThemeContext} from '../../theme';
import {HSPACING, VSPACING} from '../../theme/base';
import {CustomText} from '../static/CustomText';
import {EraserIcon, RoundedIcon} from '../static/Icon';
import {DynamicComponentProps} from './DynamicComponent';
import {Label} from './Label';
import {CustomVideoPlayer} from './media/CustomVideoPlayer';
import {NullValue} from './NullValue';

const log = new Logger('VideoUploadComponent', LogLevel.INFO);

export const VideoUploadComponent = (props: DynamicComponentProps) => {
  const stores = useContext(StoresContext);
  const theme = useContext(ThemeContext);

  const component: VideoUpload = props.component as VideoUpload;

  const [rawMedia, setRawMedia] = useState<RawMedia | null>(null);
  const [downloadURL, setDownloadURL] = useState<string | null>(null);

  // Allow pick from camera if Facility allows it, and if component allows it too
  const isPickFromCameraAllowed: boolean =
    stores?.userStore.facility?.disablePickPhotoFromCamera !== true &&
    component.enablePickFromCamera;

  // Allow pick from camera roll if Facility allows it, and if component allows it too
  const isPickFromRollAllowed: boolean =
    stores?.userStore.facility?.disablePickPhotoFromCameraRoll !== true &&
    component.enablePickFromRoll;

  // Allow export to camera roll if Facility allows it, and if component allows it too
  // const enableExportToCameraRoll: boolean =
  //   (stores?.userStore.facility?.enableExportToCameraRoll || false) &&
  //   component.enableExportToCameraRoll;

  log.highlight('Media :', rawMedia);

  useEffect(() => {
    const videoUpload = props.component as VideoUpload;
    setRawMedia(videoUpload.media);

    if (videoUpload.media && isLocalFile(videoUpload.media.uri)) {
      setDownloadURL(videoUpload.media.uri);
    } else if (videoUpload.media) {
      getDownloadURL(videoUpload.media.uri)
        .then(url => {
          setDownloadURL(url);
        })
        .catch(e => {
          log.warn(
            'Error when calling getDownloadURL in UseEffect : ' +
              JSON.stringify(e) +
              '\n' +
              videoUpload.media?.uri,
          );
          log.error('Error in useEffect getDownloadURL()', e);
        });
    }
  }, [props]);

  const _handleOpenMediaPicker = async (
    options: any,
    mode: MEDIA_PICKER_MODE,
  ) => {
    if (mode === MEDIA_PICKER_MODE.PICKER) {
      ImagePicker.openPicker(options)
        .then(userSelection => {
          _onMediaSelected(userSelection);
        })
        .catch(_err => {
          log.debug('User cancelled video selection');
        });
    } else if (mode === MEDIA_PICKER_MODE.CAMERA) {
      ImagePicker.openCamera(options)
        .then(userSelection => {
          _onMediaSelected(userSelection);
        })
        .catch(_err => {
          log.debug('User cancelled camera');
        });
    }
  };

  const _onMediaSelected = async (userSelection: any) => {
    log.debug('User selection : ', userSelection);
    let width = userSelection.width;
    let height = userSelection.height;

    try {
      // Check if the system rotated the video
      const rotation = await getVideoRotation(userSelection.path);
      log.debug('Rotation =', rotation);
      if (Math.abs(rotation) === 90) {
        // if the video was rotated, we need to swap width and height
        width = userSelection.height;
        height = userSelection.width;
      }
    } catch (err) {
      log.warn('Error while reading video rotation :', err);
    }
    log.debug('Final dimensions :', {
      width: width,
      height: height,
    });

    if (Array.isArray(userSelection)) {
      // do nothing
    } else {
      const newMedia: RawMedia = {
        type: MEDIA_TYPE.VIDEO,
        uri: userSelection.path,
        width: width,
        height: height,
        ratio: userSelection.height / userSelection.width,
        description: '',
        tags: [],
        mime: userSelection.mime,
        duration: userSelection.duration,
      };

      setRawMedia(newMedia);
      setDownloadURL(newMedia.uri);
      stores?.patientStore.updateComponentValue(
        props.sectionKey,
        props.groupKey,
        component.key,
        newMedia,
      );
    }
  };

  const renderMediaSelector = () => {
    return (
      <View
        style={{
          ...styles.view,
          backgroundColor: theme.components?.Container.backgroundColor,
        }}>
        <CustomText
          style={{
            color: theme.forms?.Inactive.color,
            ...theme.text?.Caption,
          }}>
          {isPickFromCameraAllowed && 'Filmez'}

          {isPickFromRollAllowed && ' ou choisissez une vidéo'}
        </CustomText>
        <View style={{paddingVertical: 2 * HSPACING, ...styles.centeredRow}}>
          {isPickFromCameraAllowed && (
            <RoundedIcon
              name="film"
              type="font-awesome"
              style={{marginRight: HSPACING}}
              onPress={() =>
                _handleOpenMediaPicker(
                  {
                    multiple: false,
                    ...VIDEO_OPTIONS,
                    ...SHARED_OPTIONS,
                  },
                  MEDIA_PICKER_MODE.CAMERA,
                )
              }
            />
          )}
          {isPickFromRollAllowed && (
            <RoundedIcon
              name="collection-play-fill"
              type="bootstrap"
              style={{marginRight: HSPACING}}
              onPress={() =>
                _handleOpenMediaPicker(
                  {
                    multiple: false,
                    ...VIDEO_OPTIONS,
                    ...SHARED_OPTIONS,
                  },
                  MEDIA_PICKER_MODE.PICKER,
                )
              }
            />
          )}
        </View>
      </View>
    );
  };

  return (
    <>
      <View style={{...styles.row, paddingBottom: VSPACING}}>
        <Label>{component.label}</Label>

        {!props.disabled &&
          (rawMedia !== null ? (
            <EraserIcon
              onPress={() => {
                setRawMedia(null);
                stores?.patientStore.updateComponentValue(
                  props.sectionKey,
                  props.groupKey,
                  component.key,
                  null,
                );
              }}
            />
          ) : (
            <NullValue />
          ))}
      </View>
      {rawMedia && downloadURL ? (
        <CustomVideoPlayer
          width={rawMedia.height}
          height={rawMedia.width}
          videoWidth={rawMedia.width}
          videoHeight={rawMedia.height}
          duration={rawMedia.duration}
          uri={downloadURL}
        />
      ) : !props.disabled ? (
        renderMediaSelector()
      ) : (
        <NullValue />
      )}
    </>
  );
};

const styles = StyleSheet.create({
  view: {
    paddingBottom: HSPACING,
    justifyContent: 'center',
    alignItems: 'center',
  },
  row: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: VSPACING,
  },
  centeredRow: {
    borderColor: 'red',
    // borderWidth: 1,
    width: '100%',
    flexDirection: 'row',
    justifyContent: 'flex-start',
  },
  halfSize: {
    width: '50%',
  },
  icons: {
    // position: 'absolute',
    // top: 4 * VSPACING,
    // right: 3 * HSPACING,

    flexDirection: 'row',
    borderColor: 'red',
    // borderWidth: 1,
  },
  imageButton: {
    paddingRight: 1.5 * HSPACING,
  },
});

```

### Utilisation dans un projet ❌ / ✔️

[lien github](...)

Description :

### Utilisation en production si applicable❌ / ✔️

[lien du projet](...)

Description :

### Utilisation en environement professionnel ❌ / ✔️

Description :

## 🌐 J'utilise des ressources

### Titre

- lien
- description

## 🚧 Je franchis les obstacles

### Point de blocage ❌ / ✔️

Description:

Plan d'action : (à valider par le formateur)

- action 1 ❌ / ✔️
- action 2 ❌ / ✔️
- ...

Résolution :

## 📽️ J'en fais la démonstration

- J'ai ecrit un [tutoriel](...) ❌ / ✔️
- J'ai fait une [présentation](...) ❌ / ✔️
