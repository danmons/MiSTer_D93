# MiSTer FPGA D93 whitepoint filter

## How to use this repo

Edit the file on your MiSTer's SD card located at `/media/fat/downloader.ini`, and at the very bottom place the following:

```ini
[danmons/MiSTer_D93]
db_url = https://raw.githubusercontent.com/danmons/MiSTer_D93/db/db.json.zip
```
Once done, locate the `update_all.sh` script and run it. 

## Where files will appear

This will create a series of "Gamma" .txt files for use with MiSTer's video processing, located inside the `/media/fat/Gamma/D93` folder. 

## How to use these files inside of MiSTer

These filters will only work when MiSTer's video scaler is in operation.  "Direct video" unscaled modes over eiher the HDMI or analogue (VGA, YPbPr, etc) out ports does not use the video scaler, so this filter will not apply. 

When the video scaler is in operation, in any running core that supports filters, you can can open the MiSTer menu, navigate to the `System` page, then to `Video processing`, and there you'll see a `Gamma correction` option.  Turn this to "On", and then select any of the filters on offer inside the "D93" folder. 

## What these filters do

Most colour displays, including older CRTs and Plasma TVs, as well as newer LCD and OLED screens, generate colours using Red, Green and Blue light. 

There is no such thing as "white light".  White is how our eyes and brains percieve a mix of wavelengths.  The ratios in which we mix these values are measured as a "colour temperature" in Kelvin (or the letter K for short). 

Most modern display systems use the "D65" standard for white light.  i.e.: 6504K (previously 6500K, however it was corrected some time after to a more accurate 6504K). 

Back in the 20th century, particularly in Japan, many screen shipped with a white point that was slightly more blue, or "cooler".  Confusingly, this is 9305K (in the same way that hotter stars are more blue, and colder stars are more red, but we refer to red as "hot" and blue as "cold").  Like D65's "6504 vs 6500", this was previously 9300K, and later corrected to a more accurate 9305K. 

This is referred to as "D93".  You may see reference to "D93", or numbers like "9300" on certain older displays, especially when in colour calibration or colour temperature menus.  Many old CRTs such as PC VGA displays or Sony PVM broadcast monitors would offer "6500" and "9300" preset values for their colour temperature settings. 

Particularly in Japan in the 80s and 90s, many films, TV shows, video games and anime were created, checked, edited and mastered on D93 displays, and many consumer televisions in Japan shipped with this configuration.  When considering the intent of what artists of the era wanted to display, it's worth comparing these filters with your TV's default colours to see how the presentation differs.  

At a very high level, the image will appear bluer/cooler for the most part.  However differences can be far more subtle.  Games like Super Mario Bros on the NES/Famicom have their famously "purple" sky, however under D93 this looks far more blue.  Systems with more complex colour palettes like the SuperFamicom/SNES, MegaDrive/SNES, PlayStation and Nintend64 are all interesting to compare D65 and D93 whitepoints across many titles, especially games of Japanese origin.  My personal favourite video game of all time, "Super Metroid" on the Super Nintendo, feels far more grim and foreboding under D93, with greyer skies above ground, and eerier caverns below ground. 

You can mix these with other horizontal and vertical blur/sharpen filters, scanline filters, shadow mask filters, etc.  On my calibrated Sony Bravia A80J OLED TV, I personally quite enjoy using the Preset option `Display Specific` -> `Sony PVM`, and then changing the `Gamma correction` option from the default `gamma_110.txt` to something like `D93_090_percent_gamma_2.4.txt`.  See below for what these values mean. 

When first selecting a D93 filter, this can seem quite jarring at first.  Allow yourself to continue playing the game or viewing the content for a few minutes at least.  Your brain's natural "colour constancy" will adapt quickly, and then you can see the same jarring effect as you switch back to the default D65. 

## Why all the different files?

For a visual explanation of the challenges this presents, see [this notebook](https://github.com/danmons/colour_matrix_adaptations/blob/main/jupyter/compare_gamut.ipynb). 

Because D93 and D65 are a different mix of the R, G and B primaries, the extreme end towards "white" in D93 falls outside of the colour volume that D65 can show.  This means that colours with high blue values will clip.  Typically this isn't a problem, as there are rarely a lot of colours on screen that have blue values towards 100%.  All the same, older games with limited colour palettes can often use lots of bold and fully saturated colours, so this can be annoying for certain games. 

As such, several scaled-back versions of the files are on offer.  Scaled back to approximately 74.54%, D93 fits entirely inside of D65.  The downside here is that you lose some brightness.  However if blue clipping is of concern to you, select more agressively scaled options, and perhaps increase your screen's brightness to compensate.  On offer are files scaled from 75% to unscaled 100%, in increments of 5% (with filenames starting with something like `D93_075_percent_` to indicate the scale amount). 

In addition, these files also have gamma correction options.  The internal processing of the video scaler is assumed to use the sRGB colour space and a standard gamma of 2.2.  If that sounds confusing, don't worry too much about the exact colour science behind this.  Just consider that any file I provide here with `_gamma_2.2.txt` at the end has the gamma unchanged from the default (i.e.: no change in gamma between input and output).

A lower gamma value (for example, files that end in `_gamma_2.0.txt` or `_gamma_1.8.txt`) will appear to have their lower to mid range values a little brighter, and overall image contrast a little lower.  However certain old computers like very old Apple equipment often shipped with a screen that had a native gamma of 1.8, so running old Apple computer cores on MiSTer may look better under these gamma settings depending on the content/games/applications you have loaded.  These lower gamma settings can also help to offset some of the brightness loss if you are using a heavily scaled option, or lift darker areas to be more visible.  For bright games, these settings may appear a little "washed out". 

A higher gamma value (for example, files that end in `_gamma_2.4.txt` or `_gamma_2.6.txt`) will appear to have their lower to mid range values a little darker, and overall image contrast a little higher.  Many Sony PVMs in particular had a native gamma a little higher than the 2.2 standard, and as such settings like gamma 2.4 can be more pleasant, especially for CRT enthusiasts.  MiSTer's `gamma_110.txt` set as default for the `Sony PVM` preset is quite close to gamma 2.4, and the `gamma_113.txt` set as default for any of the `SNES` presets is somewhere closer to gamma 2.6.   

Which scale and gamma setting you choose is entirely up to you, and will depend very much on the core and even individual game you're playing.  All of the files offer a D93 whitepoint, and simply change how brightness, contrast and gamma appear.  You can use tools like the [240p Test Suite](https://junkerhq.net/xrgb/index.php?title=240p_test_suite) to bring up various colour, PLUGE and other test patterns inside your MiSTer cores to help you select your preferred option per core. 

## For best results...

All of these files assume your display is roughly calibrated and close to modern standards such as sRGB (for PC monitors), or Rec.709 (for HD/UHD/4K TVs running in SDR mode), all with a D65 white point. This sounds scary, but don't panic... 

It's worth noting that many displays ship with defaults that are designed to fool unsuspecting customers that they are "brighter" and "have more pop" in order to extract their money.  Often the default "Standard", "Dynamic" or "Vivid" settings are grossly inaccurate.  However most displays also ship with some sort of colour control settings to undo this nonsense.  Displays made in the last few years typically ship with settings such as "Filmmaker Mode", or colour settings such as "Expert", "Pro" or "Cinema", all of which tend to be much closer to the correct white point and gamma settings.  Good quality display review sites like [RTings](https://rtings.com) or YouTube channels like [HDTVTest](https://www.youtube.com/@hdtvtest/videos) are great resources run by professional calibrators for locating your specific model display and setting them to the right option in the menus.  Even without access to expensive calibration equipment, the settings they recommend are pretty close to decent quality standards, and will get you closer to seeing accurate colours. 

## Limitations

This is not a proper 3D LUT. MiSTer FPGA's video scaler can only interact with colours on the gamma curves.  R/G/B primaries cannot be changed, nor can colours be corrected fully like a 3D LUT would offer.  These are purely gamma curves that can operate on R/G/B levels to shift the white point.  If you refer to my [Colour Matrix Adaptations](https://github.com/danmons/colour_matrix_adaptations) repo, you'll see more complex colourspace adaptations there to more accurately simulate various colour spaces and measured displays like the Sony PVM.  MiSTer FPGA can't do this complex level of colour correction, and can only do basic white point correction. 

MiSTer is also limited to SDR gamuts.  It does have a faux "HDR" setting, but this is quite inaccurate (it doesn't map to any known HDR standard, and requires manually adjusting your display to make it work), and doesn't actually impact any of the image processing (it simply tells your TV to be in HDR mode, which helps to bump the peak brightness value).  All the actual image processing and colour correction is still done in SDR mode internally, so there's no advantage for D93 adaptation. 

If more complex and accurate colour correction is of interest to you, consider devices like the [RetroTINK-4K](https://www.retrotink.com/product-page/retrotink-4k) which has full matrix correction available to simulate a wide variety of displays, and for which I have contributed various conversion matrices to (generated from my repo above).  It can also offer full Rec.2020 output with proper HDR and wide colour gamut, which means a wide variety of SDR colour standards and colourspaces with a D93 whitepoint can be displayed correctly without clipping or the need to scale. 

## When I select your D93 settings, I can't see any colour difference! 

One of two things is happening: either you've got a setting wrong somewhere (you're not in scaler mode, for example), or you've just discovered you're partially colour blind. You wouldn't be the first. 

## But I don't care about your silly colour pedantry, and just want my TV to pop!

This is also fine.  You are more than welcome to play video games and watch TV in whatever terrible, gaudy, overblow and oversaturated setting you want.  We can't be friends, but I also won't stop you from making your own subjective choices. 
