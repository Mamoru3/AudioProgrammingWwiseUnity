# AudioProgrammingWwiseUnity
A game that focuses on audio programming, developed using Unity and Wwise that features spatialisation and orientation, offline code process and other audio techniques

AUDIO RECORDING/SOUND DESIGN
The game features different sounds and background music. Most of the sounds have been recorded in 
the audio studio in Abertay University, with other two students: Hamish Hill and Martin Paule. The 
background music instead, has been recorded at home along with the harpoon shot sound.
Audio Recording: Sounds
Five different tracks have been recorded in the studio using different props to recreate specific sounds. 
The software used in the studio to record the tracks was Reaper. Audacity has then been used to trim 
the original tracks and extrapolate the different single sounds needed for the game. A remarkably 
important aspect is that different samples of certain sounds have been recorded in the studio 
(footsteps’ sounds) not only to alternate between them in the game scene, but also to have different 
tracks to choose from if certain ones are not high enough quality.
Some sounds have not been recorded in the studio and have been obtained in
https://pixabay.com/sound-effects/. The sounds that have not been personally recorded are: BarsOpen, 
Bell, ChestOpen, Waterfall, Door3Unlock, GruntSound and ObjectDrop. 
Audio Recording: Music
The background music has been recorded at home using an electric guitar and Amplitube 5 as 
recording software and “SRV Clean and Solo” as processing effect. The background music consists of 
two tracks, one track where the backing chords are played and another where the melody is played. 
The backing track’s volume has been decreased to allow the player to hear the melody over the chords 
better.

CODING AN AUDIO EFFECTS PROCESS
An offline audio effects process was developed to add personally coded effects to certain audio tracks. 
It features: Amplification, Filters (Low and High), Normalisation, Distortion and Delay (Single and 
Multiple). 
The amplification effect works by multiplying the AudioData vector at the iteration position by the 
amplification value. The loop iterates through the number of frames (number of samples by the 
number of channels). This loop is used in all the other effects as well.
The normalisation effect gets the loudest value in the buffer to obtain a multiplier. 
(1.0f/loudestSample). The whole buffer is then multiplied by the multiplier to achieve the 
normalisation.
The filter effect can either apply a low pass filter or high pass filter. In the low pass filter the cut-off 
of the filter is decided (0 to 1). To achieve the high pass filter, the low pass-filtered output is 
subtracted from the input.
To apply the distortion to the input, the input is squared by a fifth-degree polynomial. The way this 
distortion works is it takes the input sample X and transforms it to an output sample Y, the best result 
has been achieved using a fifth-degree polynomial over a second, third or fourth degree. In this case, 
the equation can be written as Y = (5/2)X – (1/2)X^5, which becomes Y = 2.5X – 0.5X^5. Further 
information can be found in the following web page: https://www.musicdsp.org/en/latest/Effects/114-
waveshaper-simple-description.html.
Finally, the delay effect implemented features a single effect or a multiple effect, which indicates the 
number of repetitions of the delay. In the single delay, a ring buffer is used. Its size is the 
samplerate*delayTime. While iterating through the number of frames, if the iterator position is bigger 
than the ring buffer size, the current position of the buffer minus the ring buffer size is safely added in 
the ring buffer. In the opposite condition the same position is accessed by adding the number of 
frames plus the current position of the buffer minus the ring buffer size. This is done to avoid going 
out of the bounds of the audioData buffer. Finally, the ring buffer is attenuated by a constant value, 
and then added to the original buffer.
In the multiple delay, a different approach is taken. An offset index for the original buffer is created, 
its size is the samplerate*delayTime (as the previous RingBufferSizeVariable). Feedback and delay 
level variables are also created and initialised. While looping through the number of frames of the 
buffer, the feedback is added to the buffer at the offset index position by multiplying the two, then the 
buffer at the iteration position multiplied by the delayLevel is also added to the buffer at the 
offsetIndex to achieve the delay. The buffer at the offset index is then added to the ringBuffer, which 
now holds the delay. The current index and the offset index positions are then updated. 
Similarly to the single delay, the attenuated ring buffer is then added to the original buffer.
Further information about the distortion implementation and the delay effect can be found in the book 
“Designing Audio Effect Plugins in C++” (Pirkle, 2019). 
IMPLEMENTING AUDIO EFFECTS IN A GAMES CONTEXT
Here the different sounds implemented and their audio effects will be listed, along with what software 
has been used to add the different effects. It is important to mention that most sounds’ volume has 
been adjusted in Wwise. This was done to create a homogeneous sound-environment in the game and 
therefore, the files that have no modifications other than a change in volume amplitude will not be 
mentioned.
• The background music features a single delay effect and distortion effect, obtained through 
the offline code application personally developed. 
• The footsteps’ sounds are contained in their own respective random container to shift between 
the different tracks. These random containers are then encapsulated in a Switch container. All 
the footsteps’ pitch is randomised (in a range from -300 to 300) every time a single footstep 
sound is played, to give the player a feeling of real randomness of steps on the surfaces and 
avoid repetitiveness. The grass and hard surface footsteps have a medium amount of low pass 
filter applied, to make the sound full and darker. The sand footsteps have low pass and high 
pass filter.
• The Bell audio was trimmed, then a delay effect was applied using the offline code process.
• The CoinCollection sound has a random amount of low pass filter using Wwise, to play a 
slightly different sound every time the player collects coins from a chest.
• The Door1Unlock sound has some low pass filter applied using Wwise
• The Grunt sound has a random pitch (in a range from -300 to 300) and a random low pass 
filter level (from 0 to 50)
• The HarpoonShot sound was realised by energetically blowing air on the microphone. To 
achieve a similar sound of a harpoon shooting, the pitch as then been reduced by 800 and 
some low pass filter was applied (50).
• The HouseDoorOpen sound pitch was reduced (-200).
• The Jump and JumpLand sounds pitch were reduced (-300) and low pass filter was applied 
(50 for the jump and 70 for the JumpLand).
• The ObjectDrop’s pitch is randomised (-200 to 200).

COMPRESSED AUDIO FILE FORMATS
Each audio file present in the game is compressed in the appropriate file format. The conversion 
process is done through Wwise. Short audio tracks are compressed to wav to preserve maximum 
quality while keeping full size. Even if the full size of the file is kept, it does not affect the final size 
of the project considering the original size of the files.
Being longer tracks, the fireplace and the waterfall sounds are converted to vorbis, which uses more 
processing power but reduces the size of the files considerably.
Finally, only the background track has been converted to ADPCM being of medium size.
While in this specific instance the project is not of considerable size, all the files could be kept in the 
.wav compression and the final size of the project would not considerably suffer from it. The 
meticolous choice of conversion to file formats makes the project very scalable in complexity and 
size, particularly on the audio side of the application.

AUDIO MIDDLEWARE
The audio middleware used in this application is Wwise. Different features of this powerful audio 
middleware have been exploited including containers (random and switch), file format conversion, 
game syncs to switch what footsteps are being played depending on what surface the player is 
walking on, distance-based attenuation and custom attenuations using cone and standard attenuation. 
Finally, spatialisation is implemented to achieve orientalised and positioned audio in the game. While 
most of the effects could have been achieved in the same way in the offline code process (especially 
applying low filter), Wwise makes the same process quicker and easier to tweak to reach the right 
values. It is important to mention that some sounds are played from the scripts, and some of them are 
played using pre-made Wwise components. More specifically, sounds that require certain game 
conditions and are not spatialised are played in code, whereas sounds that play in the background
(fireplace, waterfall) or can be started by collision from the player (bell) are implemented using the 
pre-existing Wwise scripts.

SPATIALISATION
As previously mentioned, only certain sounds use spatialisation techniques. This is due to the 
processing power required to spatialise sounds, especially if they also use the camera orientation.
More specifically, the sounds that are spatialised and use camera orientation are: fireplace, waterfall 
and bell. 
Each of the three spatialised sounds use different attenuation curves depending on how intense the 
sound realistically is.
As an example, the waterfall sound will be considered. It has a maximum distance of 200, its low pass 
filter increases and its volume decreases as the player gets farther from the waterfall. They are 
therefore indirectly proportional. In this case, the cone attenuation is not used as the player cannot go 
behind the audio source. The bell sound features cone attenuation so that when the player goes behind 
the bell, the sound will be very faint. If the player stands in front of the bell, then the sound will be
louder and more intense.

REFLECTION/EVALUATION
Overall, the foreseen result has been achieved and surpassed. The most challenging feature to 
implement has certainly been the multi-delay effect and the audio spatialisation due to the complexity 
of these techniques. While at first Wwise seemed an unfriendly software, after getting familiar with it, 
it allowed the implementation of different advanced audio techniques very easily. Getting familiar 
with Wwise is also a great skill that can be extremely useful in the industry given its popularity. Given 
more time, more offline code effects could have been implemented (chorus, reverb) and an 
improvement of the existing ones could be performed. The game’s software architecture and Wwise’s 
implementation of features make the project considerably scalable and ready to be extended without 
radical changes.
