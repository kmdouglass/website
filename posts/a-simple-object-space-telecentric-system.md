<!--
.. title: A Simple Object-Space Telecentric System
.. slug: a-simple-object-space-telecentric-system
.. date: 2024-03-11 08:59:17 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: true
-->

# Object-space telecentricity

I have been working on a software package recently for optical systems design. The process of building the package has proceeded like this:

1. Think of a particular case that I want to model; for example an infinite conjugate afocal system
2. Implement it in the code
3. Discover that the code doesn't work
4. Create a test case that helps debug the code
5. Repeat

I am modeling a telecentric lens in the current iteration of this loop. To keep things simple, I am limiting myself to an [object-space telecentric system](https://en.wikipedia.org/wiki/Telecentric_lens#Object-space_telecentric_lenses). This was more challenging than I expected. In part, the reason is that I was trying to infer whether a system was or was not telecentric from the lens prescription data and a ray trace, which has two problems:

1. I need to do a floating point comparison between two numbers to say whether a system is telecentric. Either the chief ray angle in object-space has to be zero or the entrance pupil must be located at infinity. Floating point comparisons are notoriously difficult to get right, and if you're doing them then you might want to rethink what you're trying to model.
2. Numerous checks are needed before we can even trace any rays. For example, I should check first whether the user placed the object at infinity. This would form the image in the same plane as the aperture stop, which does not really make sense.

I find it interesting that [Zemax addresses these problems](https://support.zemax.com/hc/en-us/articles/1500005488201-Modeling-a-lens-that-is-telecentric-in-image-space) by introducing object-space telecentricity as an extra boolean flag that forces the chief ray angle to be zero in the object-space. In other words, the user needs to know what they're doing and to specify that they want telecentricity from the beginning.

# An object-space telecentric example

I adapted the following example from lens data presented in this video: [https://www.youtube.com/watch?v=JfstTsuNAz0](https://www.youtube.com/watch?v=JfstTsuNAz0). Notably, the object distance was increased by nearly a factor of two from what was given in the video so that the image plane was at a finite distance from the lens. Paraxial ray trace results were computed by hand.

<table border="1">
    <caption>
        A simple object-space telecentric system comprising a planoconvex lens and a stop.
    </caption>
    <thead>
        <tr>
            <th scope="row">Surface</th>
            <th>0</th>
            <th>1</th>
            <th>2</th>
            <th>3</th>
            <th>4</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row">Comment</th>
            <td>OBJ</td>
            <td></td>
            <td></td>
            <td>STOP</td>
            <td>IMG</td>
        </tr>
    </tbody>
    <tbody>
        <tr>
            <th scope="row">\( R \)</th>
            <td></td>
            <td>\( \infty \)</td>
            <td>-9.750</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <th scope="row">\( t \)</th>
            <td>29.4702</td>
            <td>2</td>
            <td>15.97699</td>
            <td>17.323380</td>
            <td></td>
        </tr>
        <tr>
            <th scope="row">\( n \)</th>
            <td>1</td>
            <td>1.610248</td>
            <td>1</td>
            <td>1</td>
            <td></td>
        </tr>
    </tbody>
    <tbody>
        <tr>
            <th scope="row">\( C \)</th>
            <td></td>
            <td>0</td>
            <td>-0.10256</td>
            <td></td>
            <td></td>
         </tr> 
        <tr>
            <th scope="row">\( -\Phi \)</th>
            <td></td>
            <td>0</td>
            <td>-0.06259</td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <th scope="row">\( t/n \)</th>
            <td>29.4702</td>
            <td>1.24204</td>
            <td>15.97699</td>
            <td>17.323380</td>
            <td></td>
        </tr>
    </tbody>
    <tbody>
        <tr>
            <th scope="row">\( y \)</th>
            <td>0</td>
            <td>29.4702</td>
            <td>30.712240</td>
            <td>15.97699</td>
            <td>0</td>
        </tr>
        <tr>
            <th scope="row">\( nu \)</th>
            <td>1</td>
            <td>1</td>
            <td>-0.922279</td>
            <td>-0.922279</td>
            <td></td>
        </tr>
    </tbody>
    <tbody>
        <tr>
            <th scope="row">\( \bar{y} \)</th>
            <td>1</td>
            <td>1</td>
            <td>1</td>
            <td>0</td>
            <td>-1.084270</td>
        </tr>
        <tr>
            <th scope="row">\( n \bar{u} \)</th>
            <td>0</td>
            <td>0</td>
            <td>-0.06259</td>
            <td>-0.06259</td>
            <td></td>
        </tr>
    </tbody>
</table>

This system is shown below with lens semi-diameters of 5 mm. Note that the stop is at the paraxial focus of the lens. The rays in the sketch cross the axis before the stop because of spherical aberration.

<svg viewbox="0, 0, 1344, 150" width="1344" height="150" fill="none" stroke="black" xmlns="http://www.w3.org/2000/svg"><path d="M 632.646354675293 142.5 L 632.646354675293 142.5 L 632.646354675293 142.5 L 632.646354675293 135.39473819732666 L 632.646354675293 128.2894731760025 L 632.646354675293 121.18420815467834 L 632.646354675293 114.078946352005 L 632.646354675293 106.97368454933167 L 632.646354675293 99.86841952800751 L 632.646354675293 92.76315450668335 L 632.646354675293 85.65789270401001 L 632.646354675293 78.55263090133667 L 632.646354675293 71.44736909866333 L 632.646354675293 64.34210085868835 L 632.646354675293 57.236839056015015 L 632.646354675293 50.131577253341675 L 632.646354675293 43.0263090133667 L 632.646354675293 35.92104721069336 L 632.646354675293 28.81578540802002 L 632.646354675293 21.71052360534668 L 632.646354675293 14.60526180267334 L 632.646354675293 7.5 L 632.646354675293 7.5 L 641.0208705067635 7.5 L 641.0208705067635 7.5 L 644.972696185112 14.60526180267334 L 648.3765449523926 21.71052360534668 L 651.27783036232 28.81578540802002 L 653.7113556861877 35.92104721069336 L 655.7038663029671 43.0263090133667 L 657.275763630867 50.131577253341675 L 658.4422525763512 57.236839056015015 L 659.2141510248184 64.34210085868835 L 659.5984016060829 71.44736909866333 L 659.5984016060829 78.55263090133667 L 659.2141510248184 85.65789270401001 L 658.4422541856766 92.76315450668335 L 657.275763630867 99.86841952800751 L 655.7038679122925 106.97368454933167 L 653.7113556861877 114.078946352005 L 651.2778335809708 121.18420815467834 L 648.3765481710434 128.2894731760025 L 644.972696185112 135.39473819732666 L 641.0208705067635 142.5 L 641.0208705067635 142.5 L 632.646354675293 142.5 Z" stroke="black" stroke-width="1" stroke-linejoin="bevel" fill="none"></path><path d="M 632.646354675293 142.5 L 632.646354675293 142.5 L 632.646354675293 135.39473819732666 L 632.646354675293 128.2894731760025 L 632.646354675293 121.18420815467834 L 632.646354675293 114.078946352005 L 632.646354675293 106.97368454933167 L 632.646354675293 99.86841952800751 L 632.646354675293 92.76315450668335 L 632.646354675293 85.65789270401001 L 632.646354675293 78.55263090133667 L 632.646354675293 71.44736909866333 L 632.646354675293 64.34210085868835 L 632.646354675293 57.236839056015015 L 632.646354675293 50.131577253341675 L 632.646354675293 43.0263090133667 L 632.646354675293 35.92104721069336 L 632.646354675293 28.81578540802002 L 632.646354675293 21.71052360534668 L 632.646354675293 14.60526180267334 L 632.646354675293 7.5" stroke="black" stroke-width="1" stroke-linejoin="miter" fill="none"></path><path d="M 875.3357162475586 142.5 L 875.3357162475586 142.5 L 875.3357162475586 81.75" stroke="black" stroke-width="1" stroke-linejoin="miter" fill="none"></path><path d="M 875.3357162475586 68.25 L 875.3357162475586 68.25 L 875.3357162475586 7.5" stroke="black" stroke-width="1" stroke-linejoin="miter" fill="none"></path><path d="M 641.0208705067635 142.5 L 641.0208705067635 142.5 L 644.972696185112 135.39473819732666 L 648.3765481710434 128.2894731760025 L 651.2778335809708 121.18420815467834 L 653.7113556861877 114.078946352005 L 655.7038679122925 106.97368454933167 L 657.275763630867 99.86841952800751 L 658.4422541856766 92.76315450668335 L 659.2141510248184 85.65789270401001 L 659.5984016060829 78.55263090133667 L 659.5984016060829 71.44736909866333 L 659.2141510248184 64.34210085868835 L 658.4422525763512 57.236839056015015 L 657.275763630867 50.131577253341675 L 655.7038663029671 43.0263090133667 L 653.7113556861877 35.92104721069336 L 651.27783036232 28.81578540802002 L 648.3765449523926 21.71052360534668 L 644.972696185112 14.60526180267334 L 641.0208705067635 7.5" stroke="black" stroke-width="1" stroke-linejoin="miter" fill="none"></path><path d="M 1109.2013397216797 142.5 L 1109.2013397216797 142.5 L 1109.2013397216797 135.39473819732666 L 1109.2013397216797 128.2894731760025 L 1109.2013397216797 121.18420815467834 L 1109.2013397216797 114.078946352005 L 1109.2013397216797 106.97368454933167 L 1109.2013397216797 99.86841952800751 L 1109.2013397216797 92.76315450668335 L 1109.2013397216797 85.65789270401001 L 1109.2013397216797 78.55263090133667 L 1109.2013397216797 71.44736909866333 L 1109.2013397216797 64.34210085868835 L 1109.2013397216797 57.236839056015015 L 1109.2013397216797 50.131577253341675 L 1109.2013397216797 43.0263090133667 L 1109.2013397216797 35.92104721069336 L 1109.2013397216797 28.81578540802002 L 1109.2013397216797 21.71052360534668 L 1109.2013397216797 14.60526180267334 L 1109.2013397216797 7.5" stroke="#999999" stroke-width="1" stroke-linejoin="miter" fill="none"></path><path d="M 234.7986602783203 142.5 L 234.7986602783203 142.5 L 234.7986602783203 135.39473819732666 L 234.7986602783203 128.2894731760025 L 234.7986602783203 121.18420815467834 L 234.7986602783203 114.078946352005 L 234.7986602783203 106.97368454933167 L 234.7986602783203 99.86841952800751 L 234.7986602783203 92.76315450668335 L 234.7986602783203 85.65789270401001 L 234.7986602783203 78.55263090133667 L 234.7986602783203 71.44736909866333 L 234.7986602783203 64.34210085868835 L 234.7986602783203 57.236839056015015 L 234.7986602783203 50.131577253341675 L 234.7986602783203 43.0263090133667 L 234.7986602783203 35.92104721069336 L 234.7986602783203 28.81578540802002 L 234.7986602783203 21.71052360534668 L 234.7986602783203 14.60526180267334 L 234.7986602783203 7.5" stroke="#999999" stroke-width="1" stroke-linejoin="miter" fill="none"></path><path d="M 234.7986602783203 115.5 L 234.7986602783203 115.5 L 632.646354675293 115.5 L 653.2606882452965 115.5 L 875.3357162475586 69.18727111816406 L 1109.2013397216797 20.41567325592041" stroke="red" stroke-width="0.5" stroke-linejoin="miter" fill="none"></path><path d="M 234.7986602783203 75 L 234.7986602783203 75 L 632.646354675293 75 L 659.646354675293 75 L 875.3357162475586 75 L 1109.2013397216797 75" stroke="red" stroke-width="0.5" stroke-linejoin="miter" fill="none"></path><path d="M 234.7986602783203 34.5 L 234.7986602783203 34.5 L 632.646354675293 34.5 L 653.2606882452965 34.5 L 875.3357162475586 80.81272888183594 L 1109.2013397216797 129.5843267440796" stroke="red" stroke-width="0.5" stroke-linejoin="miter" fill="none"></path><path d="M 234.7986602783203 2451228.234375 L 234.7986602783203 2451228.234375 L 632.646354675293 2451193.4296875" stroke="red" stroke-width="0.5" stroke-linejoin="miter" fill="none"></path><path d="M 234.7986602783203 2451187.734375 L 234.7986602783203 2451187.734375 L 632.646354675293 2451152.9296875" stroke="red" stroke-width="0.5" stroke-linejoin="miter" fill="none"></path><path d="M 234.7986602783203 2451147.234375 L 234.7986602783203 2451147.234375 L 632.646354675293 2451112.4296875" stroke="red" stroke-width="0.5" stroke-linejoin="miter" fill="none"></path></svg>

# Remarks

At first the marginal ray trace was a bit confusing because the entrance pupil is at infinity. How can the marginal ray, which intersects the pupil at its edge, be traced when the pupil is at infinity? Then I remembered that I don't aim for the edge of the pupil when tracing the marginal ray. Instead, I launch a ray from the axis in the object plane at a random angle taking the surface with the smallest ray height as the aperture stop. (I chose a paraxial angle of 1 in the table above. Technically, this is called a pseudo-marginal ray. The real marginal ray is calculated from it by rescaling the surface intersection heights by the aperture stop semi-diameter.) Once you have the marginal ray in image space, just find its intersection with the axis to determine the image location.
