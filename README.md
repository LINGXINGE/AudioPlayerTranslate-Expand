AudioPlayerTranslate-Expand
This is the translated and extended version of AudioPlayer for **SCP: Secret Laboratory (Exiled framework)**

## Downloads:
| Framework | Version    |  Release                                                              |
|:---------:|:----------:|:----------------------------------------------------------------------:|
| Exiled    | ≥ 9.12.x   | [⬇️](https://github.com/LINGXINGE/AudioPlayerTranslate-Expand/releases/latest)|

---

## Plugins Overview
🔊Audio Playback
[Usage Method (Author Library)](https://github.com/Antoniofo/AudioPlayer)

•A setting called ``Volume`` has been added to the config, which can be adjusted to change the audio playback volume in round

•Added a custom event ``MusicPlayFinishEventArgs``

•Added a setting that allows administrators to play custom audio without permissions.

---

## Example for Event

<details>
<summary>
1. OnMusicFinish
</summary>

```C#
public static void OnMusicFinish(MusicPlayFinishEventArgs ev)
{
     Log.Debug("A music has finished");
}
```
</details>

## ⚠Note
It also requires AudioPlayer's dependencies

## Original author：Antoniofo
## Translate And Expand author：NekAtimo
If you find some issue,you can send an email to 1801665309@qq.com
