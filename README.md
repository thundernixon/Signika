# Signika
Making a variable version of Signika, from Google Fonts

![Signika VF](https://github.com/thundernixon/Signika/blob/f3ae1a5bee3735feb7f69db54833175cbb658134/dist/Signika-MM-VF-2018-10-16-16_01/signika-VF%202018-10-16%20at%2015.41.36.gif)


# Build Process

The sources can be built with FontMake, but I've put together some specific build scripts to pass the fonts through some steps that fix metadata issues.

## Step 1: Install Requirements

To operate the scripts within this repo, install requirements with:

```
pip install -r sources/scripts/requirements.txt
```

(Caveat: this installs all Python 3 dependencies I've installed for Google Fonts work. I know this is messy – next time, I'll set up a virtual environment for each project. I hope to circle back in the future and make this requirements file cleaner. If you wish to install fewer requirements, you could alternatively install requirements when/if you run into errors.)

## Step 2: Give permissions to build scripts

The first time you run the build, you will need to give run permissions to the build scripts.

On the command line, navigate to the project folder (`cd Encode-Sans`), and then give permissions to the shell scripts with:

```
chmod -R +x sources/scripts
```

The `-R` applies your permission to each of the shell scripts in the directory, and the `+x` adds execute permissions. Before you do this for shell scripts, you should probably take a look through their contents, to be sure they aren't doing anything bad. The ones in this repo simply build from the Encode Sans GlyphsApp sources.

## Step 3: Run the build scripts!

You can then build sources by running shell scripts in `sources/scripts/`.

Build the full variable font (Weight + "Negative" axes) with:

```
sources/scripts/build-full.sh
```

Build the split variable font with (just Weight axis for "normal" version):

```
sources/scripts/build-split.sh
```

Build all static instances with:

```
sources/scripts/build-statics.sh
```

# Build steps after edits to primary source

This project has a "primary" source file which has received design updates and steps of outline QA. However, the designspace of this primary source is setup in such a way that it is not possible to export directly from it to a variable font, via FontMake. This is because it has just two weight masters, but exports to a 2-axis design, with a "Negative" axis that is derived from a smaller shift to the overall weight of fonts (similar to a Grade axis, but without maintaining a fixed width for letters).

Due to [current limitations in remote scripting for GlyphsApp](https://forum.glyphsapp.com/t/instance-as-master-through-core-api/10502/12), if you wish for edits to the design to cascade into the final outputs, you must use a partially-manual build process, wherein a few processing steps are done to make "build-ready" sources. These steps are as follows, in GlyphsApp:

1. Use "Instance as Master" to make masters out of the Light, Bold, Negative Light, and Negative Bold instances
1. In the Masters, delete the old "Light" and "Bold" Masters (their weights are outside the ultimate exporting weights)
1. Add "Negative" to the new negative master names
1. Add a custom parameter to the font for "Axes," and set these to "Weight" (tag: `wght`) and "Negative" (tag: `NEGA`).
1. Set both Light masters to Weight values of `50` and both Bold masters to a Weight value of `920`
1. Set Negative masters to Negative values of `-1` and non-negative masters to Negative values of `0`.
1. Go through Instances, setting the Negative instances to the same Weight values as their non-negative counterparts (`50, 360, 650, 920`) and their Negative values as `-1`. Set non-negative masters to a Negative value of `0`. 

The font now has a build-ready rectangular designspace.

(I may convert the partially-completed remote script to a Glyphs Python Script, as this would handle all steps required. However, in the interest of time, I am using manual methods for the time being.)

# Variable font upgrade project documentation

Notes were taken throughout the variable font upgrade project and added to the [docs](/docs) directory. I tend to take notes while working anyway, in order to think through problems and record solutions for later reference. In this project, I have included these in the repo so that others might find references to solve similar problems, especially because variable font-making processes are relatively new, and there is a general scarcity of online knowledge on font mastering. Because they were often made alongside work, the notes can at times be a bit disjointed. Hopefully they are still helpful to others! 

If you have any questions about the project or the notes, feel free to [file an issue](/issues) or to reach out to Stephen Nixon via Twitter ([@thundernixon](https://twitter.com/thundernixon)) or other social media (typically also @thundernixon).