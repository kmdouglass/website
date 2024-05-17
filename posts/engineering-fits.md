<!--
.. title: Engineering Fits
.. slug: engineering-fits
.. date: 2024-03-26 11:48:49 UTC+01:00
.. tags: fits, 3d printing
.. category: manufacturing
.. link: 
.. description: The basic theory of engineering fits
.. type: text
.. has_math: true
-->

I have been working on some optomechanical parts that require a hole-and-shaft style mating. During their design, I realized I really didn't have any theoretical background on how big the holes and shafts should be so that they fit together. This lead me to do some basic research into **engineering fits**.

# Engineering Fits

According to <cite>Building Scientific Apparatus, 4th ed.[1]</cite>, fit should be specified when the absolute size of two mating parts is not important, but the clearance between them is critical.

To understand fits, it helps first to think in terms of active surfaces and tolerances.

An **active surface** is a region where two surfaces touch and either move against each other or have a static fit [2]. (Interestingly, an active surface is really two physical surfaces by this definition.) The tolerances on the size of two mating parts determines the type of fit. An example of the tolerances on a hole-and-shaft assembly is shown below.

<svg
   width="150mm"
   height="100mm"
   viewBox="0 0 150 100"
   version="1.1"
   id="svg5"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:svg="http://www.w3.org/2000/svg">
  <defs
     id="defs2" />
  <g
     id="layer1">
    <g
       id="g17422"
       transform="translate(4.3342147,0.28437662)">
      <g
         id="g18330"
         transform="translate(7.3550761,0.97463966)">
        <rect
           style="fill:#a8a8a8;fill-opacity:1;stroke:none;stroke-width:0.0499999;stroke-linecap:square;stroke-dasharray:0.0999994, 0.199999;stroke-dashoffset:0;stroke-opacity:1"
           id="rect8681"
           width="61.416935"
           height="6.1849809"
           x="-1.2360383"
           y="24.001053"
           ry="2.3741585e-07" />
        <rect
           style="fill:#a8a8a8;fill-opacity:1;stroke:none;stroke-width:0.0499999;stroke-linecap:square;stroke-dasharray:0.0999994, 0.199999;stroke-dashoffset:0;stroke-opacity:1"
           id="rect8943"
           width="61.416935"
           height="6.1849809"
           x="-1.2359573"
           y="67.295868"
           ry="2.3741585e-07" />
        <rect
           style="fill:none;fill-opacity:1;stroke:#000000;stroke-width:0.199999;stroke-linecap:square;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:1"
           id="rect8621"
           width="61.416935"
           height="92.190269"
           x="-1.2359573"
           y="2.645849"
           ry="1.7694015e-07" />
        <path
           style="fill:none;fill-opacity:1;stroke:#000000;stroke-width:0.199999;stroke-linecap:square;stroke-dasharray:0.399999, 0.799999;stroke-dashoffset:0;stroke-opacity:1"
           d="M 60.180883,27.093527 H -1.2360264"
           id="path8677" />
        <path
           style="fill:none;fill-opacity:1;stroke:#000000;stroke-width:0.199999;stroke-linecap:square;stroke-dasharray:0.399999, 0.799999;stroke-dashoffset:0;stroke-opacity:1"
           d="M 60.18099,70.388445 H -1.2360264"
           id="path8679" />
        <rect
           style="fill:#a8a8a8;fill-opacity:1;stroke:none;stroke-width:0.199999;stroke-linecap:square;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:1"
           id="rect9673"
           width="61.708443"
           height="6.1849813"
           x="66.149086"
           y="62.016087"
           ry="2.3741585e-07" />
        <rect
           style="fill:#a8a8a8;fill-opacity:1;stroke:none;stroke-width:0.199999;stroke-linecap:square;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:1"
           id="rect9677"
           width="61.708443"
           height="6.1849813"
           x="66.149086"
           y="31.09115"
           ry="2.3741585e-07" />
        <rect
           style="fill:none;fill-opacity:1;stroke:#000000;stroke-width:0.199999;stroke-linecap:square;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:1"
           id="rect9671"
           width="61.708359"
           height="30.924906"
           x="66.149086"
           y="34.183422"
           ry="2.3741585e-07" />
        <g
           id="g18315"
           transform="translate(-24.896546)">
          <rect
             style="fill:#a8a8a8;fill-opacity:1;stroke:none;stroke-width:0.2;stroke-linecap:square;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:1"
             id="rect16362"
             width="9.260376"
             height="3.6321862"
             x="90.945633"
             y="88.398582"
             ry="1.394246e-07" />
          <text
             xml:space="preserve"
             style="font-size:4.23333px;font-family:Arial;-inkscape-font-specification:'Arial, Normal';text-align:center;text-anchor:middle;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.0499999;stroke-linecap:square;stroke-linejoin:bevel;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:1"
             x="118.46138"
             y="91.428886"
             id="text12648"><tspan
               id="tspan12646"
               style="font-size:4.23333px;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.05"
               x="118.46138"
               y="91.428886">Tolerance Ranges</tspan></text>
        </g>
        <path
           style="fill:#000000;fill-opacity:1;stroke:#000000;stroke-width:0.199999;stroke-linecap:butt;stroke-linejoin:round;stroke-dasharray:none;stroke-dashoffset:0;stroke-opacity:1"
           d="M 66.149086,49.64587 H 52.638712 l 0.728979,-0.628177 v 1.351618 L 52.638712,49.64587"
           id="path16428" />
      </g>
    </g>
  </g>
</svg>

## Fit definitions

In this context, we can define three types of fits:

1. _Clearance fits_ : Tolerance zones do not overlap
2. _Transition fits_ : Tolerance zones partially overlap
3. _Interference fits_ : Tolerance zones fully overlap

These fits exist on a continuum and are not neatly distinguished in practice. The continuum can be seen by plotting the force required for mating vs. the allowance. The allowance in this context can be defined as follows[3]:

\\[ \text{allowance} = \text{smallest hole} - \text{largest shaft} \\]

### Clearance fits

1. _Sliding fit_ : Some lateral play
2. _Running fit_ : More fricition, but more accurate motion

### Transition fits

1. _Keyring fit_ : Slight force required for mating and easy to remove
2. _Push fit_ : More force required; possible to remove by hand

### Interference fits

1. _Force fit_ : Hand tools likely required for mating
2. _Press fit_ : Requires more force, likely using a press

[1]: https://www.cambridge.org/core/books/building-scientific-apparatus/52BB9BC3EDF3A8F604EF95D83901AA00
[2]: https://formlabs.com/eu/blog/3D-printing-tolerances-for-engineering-fit/
[3]: https://waykenrm.com/blogs/difference-between-tolerance-and-allowance/
