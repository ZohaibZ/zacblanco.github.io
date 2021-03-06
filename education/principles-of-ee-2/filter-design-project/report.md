---
layout: page
title: Filter Design Project
permalink: /filter-design-project/
mathjax: true
---

# Filter Design Project

## Calculating the Problem Number

Name: Zachary Blanco

- $$ F_1 $$: z = 26
- $$ F_2 $$: a = 1
- $$ F_l $$: y = 25
- $$ L_1 $$: b = 2
- $$ L_1 $$: l = 12
- $$ L_l $$: o = 15


### Calculation for Problem Selection

$$ \text{Problem #} = 1 + Mod_9[26 + 1 + 25 + 2 - 12 - 15] = 1 + Mod_9[27] = 1 + 0 = 1$$

Thus from the above calculation, the problem number is 1.

## Design Problem Description

 > **Problem 1**: Design a low-pass active filter circuit such that its roll off attenuates frequency components at 7.5 kHz or more by more than 15 dB. Also, it is desired to have the passband gain positive that does not deviate from 20 dB by more than 3 dB. The passband frequency needed is 500 Hz and below.

If we analyze the problem here we can derive a few key facts that will allow us to design a transfer function and circuit which is within the constraints of the problem.

We get the following facts from the problem:

- The pass-band of frequencies ranges from 0Hz all the way to at least 500Hz
  - The magnitude in dB at this point must be equal to: $$ 20 \pm 3 dB$$
- The roll-off means that at a value of 7.5kHz that the value of the magnitude in decibels at 7.5kHz must be at least $$-15dB$$

## Feasibility Study

### Attempt at First-Order Filter Feasibility

If we try to find a first-order filter circuit then we need to have a gain, $$k$$ by which our transfer function will approximately begin at 20dB, and then by which will begin to roll off at approximately 500Hz. Whereby transfer function must lie within a $$\pm 3dB $$ range. 

The filter must also attentuate for frequencies which are greater than 7.5kHz to values of $$-15dB$$ or less.

By inspection it is possible to see that for a first order circuit to reach the required frequency at both ends of the range that it is not possible to utilize a first-order circuit.

We can see this because $$log(500) = 2.6989$$ and that $$log(7500) = 3.87506$$

We see that this is a change in powers of 10 by approximately $$1.176$$ This means the change in these two frequencies is 1.176 decades.

A first order filter has a maximum slope of $$-20dB/decade$$ the maximum that the filter can roll off from 500Hz to 7.5kHz is $$1.176\cdot 20dB=23.52dB$$ which is far less than required drop in decibels for our problem. The minimum total drop in decibels must be from 17dB to -15dB. A drop of 32dB. Thus the filter design is not possible utilizing only a first-order filter.

### Attempt at Second-Order Filter Feasibility

We should now look at the feasibility of designing a second-order filter which follows the constraints presented by the problem in (2).

Let's first attempt to cascade two first-order low pass transfer functions

Given a low pass filter with a transfer function of $$ H_1(s) = \frac{k_1}{1 + \frac{s}{\omega_{c1}}} $$ We can cascade two of these similar functions together to get the following:

$$ H(s) = H_1(s)H_2(s) = \frac{k_1}{1+\frac{s}{\omega_{c1}}} \cdot \frac{k_2}{1+\frac{s}{\omega_{c2}}} $$

This can then be simplified to:

$$ H(s) = \frac{k_1k_2}{ (1 + \frac{s}{\omega_{c1}} )(1 + \frac{s}{\omega_{c2}} ) } $$

Here we can see that we have two different cutoff frequencies, $$\omega_{c1}$$, $$\omega_{c2}$$. These cutoff frequencies determine where the rate of decrease can be either $$-20dB/\text{decade}$$ or $$-40dB/\text{decade}$$. We can select both of these cutoff frequencies in order to "squeeze" or "extend" the magnitude portion of the bode plot.

If we select a gain $$k_1k_2 = 10$$, then we set $$\omega_{c1}=\omega_{c2}$$, then we can determine that a drop in 40dB over a single decade will be able to fulfill the requirements of our transfer function which requires a minimum drop of 32dB over a range of 1.76 decades.

We can obviously see that a total drop with a slode of -40dB will allow the bode plot to fall in under the restrictions of the problem because $$\frac{-40dB}{\text{decade}} \cdot 1.176 = -47.04$$

This drop of 47.04dB is enough to fall within the constraints of the problem. Thus with a second order filter it is possible to design a filter given the constraints.

We can utilize a cascaded first-order Low Pass filter.

Take the first order filter below:

![](/assets/images/filter-design/LPF-2.png)

This filter has a transfer function, $$H(s) = \frac{1 + \frac{R_2}{R_1}}{1 + sR_3C}$$

This means the filter has a gain $$K = 1 + \frac{R_2}{R_1}$$. We can simplify the form of this to say that the transfer function is simple $$H(s) = \frac{k}{1 + \frac{s}{\omega_{c1}}}$$. If we cascade two such filters together we find that it is possible to get a transfer function which has the form

$$ H(s) = H_1(s)H_2(s) =  \frac{k_1k_2}{(1 + \frac{s}{\omega_{c1}})(1 + \frac{s}{\omega_{c2}})} $$

where $$H_1(s) = \frac{k_1}{1 + \frac{s}{\omega_{c1}}} $$ and $$H_2(s) = \frac{k_2}{1 + \frac{s}{\omega_{c2}}} $$

Given then $$k_1k_2 = K$$, from the above transfer function we find the magnitude function

$$ M(\omega) = \frac{K}{\sqrt{1 + (\frac{\omega}{\omega_{c1}})^2}\sqrt{1 + (\frac{\omega}{\omega_{c1}})^2}} $$

And with this 2nd order transfer function, we've proved that with a second order filter it is possible to design a filter which fits the specification of our problem. So, now it is up to us to find the gains, $$k_1$$ and $$k_2$$, and cutoff frequencies $$\omega_{c1}$$ and $$\omega_{c2}$$ which satisfy the constraints of our problem.

## Finding the Transfer Function of the Filter

Given the 2nd order cascaded filter below:

![](/assets/images/filter-design/2-order-LPF-labeled.png)

We can take the node voltage equations to find the transfer function with respect to the first filter block using nodal analysis.

We determine that the transfer function for the first filter block is:

$$ \frac{V_o}{V_{in}} = H_1(s) = \frac{1 + \frac{R_2}{R_1}}{1 + sR_3C_1} $$

so then if we apply the same transfer function to the components in the 2nd filter block we find the transfer function to be

$$ H_2(s) = \frac{1 + \frac{R_5}{R_4}}{1 + sR_6C_2} $$

And because these two filters are cascaded together we find the total transfer function $$H(s)$$ to be the product of the two function $$H_1(s)$$ and $$H_2(s)$$

Therefore, $$H(s) = H_1(s)H_2(s) = \frac{1 + \frac{R_2}{R_1}}{1 + sR_3C_1}\cdot \frac{1 + \frac{R_5}{R_4}}{1 + sR_6C_2} $$

If we then take the magnitude of the transfer function for $$s=j\omega$$

$$ M\left(\omega\right)=\frac{\left(\left(1+\frac{R_2}{R_1}\right)\left(1+\frac{R_5}{R_4}\right)\right)}{\sqrt{1+\left(\omega R_3C_1\right)^2}\sqrt{1+\left(\omega R_6C_2\right)^2}} $$


### Reasons for Filter Design Selection

Using the above filter allows us to design a circuit with two different cutoff frequencies which gives us extra control over the slope of the function, and in turn let's us satisfy the constraints of our problem.

Cascading these two first-order circuits also gives us control over the exact gain of the cascaded circuit which in turn allows for many more possibilities in terms of component values.

![](/assets/images/filter-design/2-order-LPF-1.png)

## Selection of Pass-Band Gain and Corner Frequencies

### Selection of Gain

According to the specifications of the problem, it is necessary for us to design a filter which has a magnitude function which approximately equal to 20dB for all frequencies which are less than the cutoff frequency.

Knowing this information we can say that:

- $$ 20 \approx 20log(M_H(\omega)), 0 < \omega \leq 500$$
- For $$\omega$$ on the order of $$10^{-2}$$ and smaller we should say that $$20 = M_H(\omega)$$

So given the magnitude of $$ M(\omega) = \frac{k_1k_2}{\sqrt{1 + (\frac{\omega}{\omega_{c1}})^2}\sqrt{1 + (\frac{\omega}{\omega_{c1}})^2}} $$

For the magnitude in decibels we take $$20\cdot log(M_H(\omega))$$.

For small values of $$\omega$$ we can say it is true that the denominator of the function contributed $$0$$ to the magnitude plot.

This gives us:

$$ 20dB = 20\cdot log(K) = 20\cdot log(k_1k_2)$$

Rearranging with some algebra gives us that:

> $$ k_1k_2 = 10 $$

This tells us that the product of the gain from each of the first-order circuits cascaded together must result in a product of $$10$$

### Selection of Passband Frequencies

To select the most appropriate passband frequencies it is necessary for us to realize two constraints

1. The magnitude, $$M_H$$ must be between 17dB and 23dB at $$\omega=500$$
  - $$\omega = 500, 17 < M_H(500) < 23 $$
2. The magnitude $$M_H$$ must be less than $$-15dB$$ at $$\omega = 7500$$
  - $$\omega = 7500, M_H(7500) < -15 $$

Given this we can estimate appropriate values for the cutoff frequencies $$\omega_{c1}$$ and $$\omega_{c2}$$.

It is possible to determine by inspection that our cutoff frequencies should both be greater than 500Hz, because if we set a cutoff frequency to 500 on a first-order filter of $$M(\omega) = \frac{K}{\sqrt{1 + \frac{\omega}{500}}}$$ the magnitude is $$\approx 17dB$$. This is exactly on our design's lower limit for magnitude, and only a first order filter. Thus from this our design must have a cutoff greater than 500.

We will start by picking a frequency for $$\omega_{c1}$$ and $$\omega_{c2}$$ which will give us a rough estimate of where our frequency should lie.

We can use the approximate asymptotic line equation at the point (500, 20):

$$ 20dB = 40 -40log(\frac{\omega}{500})$$

Using this we find $$\omega \approx 1580$$

So if we set $$\omega_{c1} = \omega_{c2} = 1580$$ we get the following

| Trial | $$\omega_{c1}$$(Hz) | $$\omega_{c2}$$ (Hz) | $$M(500)$$ (dB) | $$M(7500)$$ |
| 1 | 1580 | 1580 | 19.17dB | -7.433 dB |

Now we can see that setting the two frequencies equal is not exactly ideal, so we will attempt to vary the frequencies

| Trial | $$\omega_{c1}$$(Hz) | $$\omega_{c2}$$ (Hz) | $$M(500)$$ (dB) | $$M(7500)$$ |
| 2 | 1580 | 1200 | 18.89 | -9.744 |
| 3 | 1580 | 1000 | 18.62 | -11.29 |
| 4 | 1580 | 800  | 18.15 | -13.21 |
| 5 | 1580 | 625  | 17.44 | -15.33 |

After making iterative changes to the frequencies, we've found a pair of cutoffs $$(\omega_{c1}, \omega_{c2})$$ which satisfy the contstraints of the problem: $$(1580, 625)$$

After more trial and error I have also discovered other pairs of frequencies which work well together:

| Trial | $$\omega_{c1}$$(Hz) | $$\omega_{c2}$$ (Hz) | $$M(500)$$ (dB) | $$M(7500)$$ |
| 6 | 900 | 950  | 17.769 | -16.49 |
| 7 | 1000| 925  | 17.92 | -15.82 |


Given these six trials we can find the one which gives us the most amount of room for error when designing the circuit.

We will attempt to find the maximum of the function $$ G(\omega_{c1}, \omega_{c2}) = (M(500)-17)^2 + (-15-M(7500))^2 $$ for each trial. This function, $$G(\omega_{c1}, \omega_{c2})$$ gives us a value which maximizes the distance our magnitude cutoff at 500Hz and 7.5kHz. The higher this value is menas more ideal the two cutoff frequencies are with respect to our magnitude transfer function. However it doesn't take into account whether a value is above of below the required value so we still need to check whether the frequencies keep the magnitude in the correct location.

We will pick the one with the largest value of $$ G(\omega_{c1}, \omega_{c2})$$, but must also fit the constraints given by the problem.

Finding the maximum of the fun

| Trial | Is within constraints? |$$ G(\omega_{c1}, \omega_{c2})$$ |
| 1 | No |61.97 |
| 2 | No |31.20 |
| 3 | No |16.34 |
| 4 | No |4.55  |
| 5 | Yes|.300  |
| 6 | Yes|2.826 |
| 7 | Yes|1.517 |

Given the above table we will pick our cutoff frequencies $$(\omega_{c1}, \omega_{c2})$$

## Selecting Component Values

### Gain Resistors

We will keep the nominal values of capacitors and resistors in mind when picking our components for the circuit:

> (1, 1.2, 1.5, 1.8, 2.2, 2.7, 3.3, 3.9, 4.7, 5.6, 6.8, 8.2) × $$10^n$$

Below is a diagram of the filter with resistors and capacitors marked:

![](/assets/images/filter-design/2-order-LPF-labeled.png)


First we will pick the resistor values to acheive the desired gain for our magnitude.

We know that because we are cascading two filters together that our gain is equal to the product of the gain, $$k_1k_2$$ from each filter.

$$k_1 = 1 + \frac{R_2}{R_1}$$ and $$ k_2 = 1 + \frac{R_5}{R_4}$$

From this we also know that $$k_1k_2 = 10$$, so individually we will set the gain for each filter to be 2 and 5 respectively

- $$k_1 = 2 = 1 + \frac{R_2}{R_1}$$
- $$k_2 = 5 = 1 + \frac{R_5}{R_4}$$

$$ \frac{R_2}{R_1} = 1$$, thus 

> $$R_2 = R_1 $$

$$ \frac{R_5}{R_4} = 5 - 1 = 4 $$

> $$ R_5 = 4R_4 $$

So we will set $$R_1 = R_2 = 2.2k\Omega$$

Then we will also set $$R_4 = 2.2k\Omega$$

This gives us that $$R_5=4\cdot2.2k\Omega = 8.8k\Omega$$

We can set $$R_5$$ to $$8.8k\Omega$$ by putting 4 separate $$2.2k\Omega$$ resistors in series. The rest of the resistors are set to nominal values.

| Component | Value |
| $$R_1$$ | $$2.2k\Omega$$ | 
| $$R_2$$ | $$2.2k\Omega$$ | 
| $$R_4$$ | $$2.2k\Omega$$ | 
| $$R_5$$ | $$8.8k\Omega$$ | 

Using these values we should get an exact gain of 10.

### Cutoff Frequency Resistors and Capacitors

First we should note that in our magnitude equation we see something such as $$\omega RC = \frac{s}{\omega}$$. When calculating our cutoff we know that $$s=j\omega$$ and because $$\omega=2\pi f$$, the $$2\pi$$ ended up canceling out when doing the algebra because we didn't have to take it into account.

However now that we are select component values to match the cutoff frequencies we have to take into account that $$RC = \frac{1}{\omega} = \frac{1}{2\pi f}$$ where $$f$$ is the cutoff frequency. We need to include the $$2\pi$$ in order for our circuit to match correctly.

We will set our resistors for the filter portion of the circuit each equal to $$47k\Omega$$

So now from our transfer function we see that $$sRC = \frac{s}{\omega}$$

Given our cutoff frequencies 900 and 950 Hz we can see that $$\frac{1}{2\pi f} = RC$$

So then if we go back to the diagram we see that for $$\omega_{c1}$$ and $$\omega_{c2}$$ we will get the following equations:

1. $$R_3C_1 = \frac{1}{2\pi\cdot 900}$$
2. $$R_6C_2 = \frac{1}{2\pi\cdot 950}$$

From the equations above we can then say that:

$$C_2 = \frac{1}{2\pi\cdot 900R_3}$$
$$C_1 = \frac{1}{2\pi\cdot 950R_6}$$

So then we find that the exact values for $$C_1$$ and $$C_2$$ are $$3.763nF$$ and $$3.564nF$$ respectively

However, because we must choose nominal values, we will set $$C_1 = 3.9nF$$ because it is the closest nominal value

Then, we can get $$3.5nF$$ by putting a $$1.5nF$$ capacitor and two $$1nF$$ capacitors in parallel with one another

See below for a complete summary of our circuit's values

| Component | Nominal Value |
| $$R_1$$ | $$2.2k\Omega$$ |
| $$R_2$$ | $$2.2k\Omega$$ |
| $$R_3$$ | $$47k\Omega$$ |
| $$R_4$$ | $$2.2k\Omega$$ |
| $$R_5$$ | $$8.8k\Omega$$ |
| $$R_6$$ | $$47k\Omega$$ |
| $$C_1$$ | $$3.9nF$$ |
| $$C_2$$ | $$3.5nF$$ |


### Magnitude and Frequency Response Characteristic

The plots in this section were derived from the Matlab code below:

~~~
r1=2200;
r2=2200;
r3=47000;
r4=2200;
r5=8800;
r6=47000;
c1=3.9*10^-9; % From combination of nominal components- see report
c2=3.5*10^-9; % From combination of nominal components- see report

w1=(r3.*c1);
w2=(r6.*c2);
k1 = 1 + (r2./r1);
k2 = 1 + (r5./r4);
k=k2.*k1;

% Generate the bode plots
num = k;
den = [w1.*w2 w1+w2 1];
h=tf(num, den);
opts = bodeoptions('cstprefs');
opts.FreqUnits = 'Hz';
figure(1);
bodemag(h, opts);
figure(2);
bode(h, opts);
figure(3);
opts.Ylim = [-40 40];
bodemag(h, opts);
~~~

![](/assets/images/filter-design/response-2.png)

![](raw.githubusercontent.com/zacblanco/zacblanco.github.io/master/assets/images/filter-design/response-mag.png)

![](/assets/images/filter-design/labeled-response.png)


### Varying Component Frequency Response

We will use the same Matlab code from above, however we will pick two different component values to deviate by $$\pm 5\%$$. Below is the Matlab code for the deviation. Note that we picked to deviate $$R_3$$ and $$R_5$$

~~~
r1=2200;
r2=2200;
r3=47000.*(1.05);
r4=2200;
r5=8800.*(0.95);
r6=47000;
c1=3.9*10^-9; % From combination of nominal components- see report
c2=3.5*10^-9; % From combination of nominal components- see report

w1=(r3.*c1);
w2=(r6.*c2);
k1 = 1 + (r2./r1);
k2 = 1 + (r5./r4);
k=k2.*k1;

% Generate the bode plots
num = k;
den = [w1.*w2 w1+w2 1];
h=tf(num, den);
opts = bodeoptions('cstprefs');
opts.FreqUnits = 'Hz';
figure(1);
bodemag(h, opts);
figure(2);
bode(h, opts);
figure(3);
opts.Ylim = [-40 40];
bodemag(h, opts);
~~~


![](/assets/images/filter-design/altered-bode-2.png)

We can see that even though the resistors had slightly changed, our response still fell within the problem's constraints.

### Conversion From a Low Pass to a High Pass Filter

From the matlab code above we can see that the 3dB frequency is approximately 590Hz.

Given our transfer function:

> $$ H(s) = \frac{k_1k_2}{(1 + \frac{s}{\omega_{c1}})(1 + \frac{s}{\omega_{c2}})} $$

If we want to turn this function into once which mimics a High-pass filter it is necessary to replace all values of $$s$$ with $$\frac{\omega_o^2}{s}$$ where $$\omega_o$$ is the 3dB cutoff of 590Hz.

This would then give us:

$$ H(s) = \frac{k_1k_2}{(1 + \frac{\frac{\omega_o^2}{s}}{\omega_{c1}})(1 + \frac{\frac{\omega_o^2}{s}}{\omega_{c2}})} $$

Simplifed:

$$  H(s) = \frac{s^2k_1k_2}{(s + \frac{\omega_o^2}{\omega_{c1}})(s + \frac{\omega_o^2}{\omega_{c2}})} $$ 

$$ H(s) = \frac{s^2k_1k_2}{(s + \frac{\omega_o^2}{\omega_{c1}})(s + \frac{\omega_o^2}{\omega_{c2}})} $$

$$ H(s) = \frac{s^2k_1k_2}{s^2 + s\omega_o^2( \frac{1}{\omega_{c1}} + \frac{1}{\omega_{c2}}) + \frac{\omega_o^4}{\omega_{c1}\omega_{c2}} } $$

![](/assets/images/filter-design/high-pass-circuit.png)

The Matlab code below will generate the charts which follow for the above high pass circuit with that transfer function

~~~
r1=2200;
r2=2200;
r3=47000;
r4=2200;
r5=8800;
r6=47000;
c1=3.9*10^-9; % From combination of nominal components- see report
c2=3.5*10^-9; % From combination of nominal components- see report

w1=(r3.*c1);
w2=(r6.*c2);
w0=(590*2*pi); % 3dB frequency
k1 = 1 + (r2./r1);
k2 = 1 + (r5./r4);
k=k2.*k1;

% Generate the bode plots
num = [k 0 0];
den = [1 (w0^2.*(w1+w2)) (w0^4.*(w1.*w2))];
h=tf(num, den);
opts = bodeoptions('cstprefs');
opts.FreqUnits = 'Hz';
figure(1);
bodemag(h, opts);
~~~

![](/assets/images/filter-design/high-pass-mag.png)

![](/assets/images/filter-design/high-pass-bode.png)


### Frequency Scaling

In order to scale the cutoff frequency on this circuit we're going to need a scaling coefficient $$K_f=10$$. $$K_m$$ in this case is equal to 1.


So if we scale all of the resistors and capacitors using these two equations:

- $$ R' = K_mR $$
- $$ C' = \frac{C}{K_mK_f} $$

So using the above equations we can derive the following table

| Component | Value | Scaled Value
| $$R_1$$ | $$2.2k\Omega$$ | $$2.2k\Omega$$ |
| $$R_2$$ | $$2.2k\Omega$$ |$$2.2k\Omega$$|
| $$R_3$$ | $$47k\Omega$$  | $$47k\Omega$$|
| $$R_4$$ | $$2.2k\Omega$$ |$$2.2k\Omega$$|
| $$R_5$$ | $$8.8k\Omega$$ |$$8.8k\Omega$$|
| $$R_6$$ | $$47k\Omega$$  |$$47k\Omega$$|
| $$C_1$$ | $$3.9nF$$ | $$.39nF$$ |
| $$C_2$$ | $$3.5nF$$ | $$.35nF$$ |

So then given this table with these values we can use the following Matlab code to get a bode plot  with the scaled circuit

~~~
r1=2200;
r2=2200;
r3=47000;
r4=2200;
r5=8800;
r6=47000;
c1=(3.9*10^-9)./10; % From combination of nominal components- see report
c2=(3.5*10^-9)./10; % From combination of nominal components- see report

w1=(r3.*c1);
w2=(r6.*c2);
k1 = 1 + (r2./r1);
k2 = 1 + (r5./r4);
k=k2.*k1;

% Generate the bode plots
num = k;
den = [w1.*w2 w1+w2 1];
h=tf(num, den);
opts = bodeoptions('cstprefs');
opts.FreqUnits = 'Hz';
figure(1);
bodemag(h, opts);
~~~

![](/assets/images/filter-design/bode-mag-scaled.png)

## Conclusion 

_Note that this is prior to testing the circuit in the laboratory_

Working on this project gave me a different perspective on engineering. So far in class we have only analyzed and derived the response of circuits. Working backwards is actually quite interesting. It forced me to understand the concepts and the math behind each to be able to devise the correct circuit.

From designing exactly how the magnitude characteristic needed to be, all the way down to the circuit, I had to analyze exactly what I was doing at every single step. I needed to understand why I made the decisions for cutoffs and gain. And from each of those I had to work backwards into designing the circuit. It felt very satisfying when finally being able to pick the components in the circuit because I knew picking those components would lead to a circuit which had the exact characteristics I was looking for.

I think this project was a good change from what we have been doing in our classes and provided valuable experience in what an actual electrical engineer might be required to do on a typical project.

Looking back on the circuit I like to think I made a good decision in the type of low pass filter for my project because the design I chose separated resistors in the gain from the resistor and capacitor which controlled the frequency. This allowed me greater control over the circuit response than if the resistor and capacitor both affected frequency or gain.

However I did have issues in converting from radians per second to cycles per second (Hz) in a few cases which threw off my circuit's response when testing values and running simulations in pspice simulations. However I believe that after slight difficulty I have corrected myself and the circuit behaves as expected.


### References

- Circuit Schematics Designed using [PartSim](http://partsim.com) - `http://partsim.com`
- Ideas for low pass filter design from [Electronics-Tutorials](http://www.electronics-tutorials.ws/filter/filter_5.html) - `http://www.electronics-tutorials.ws/filter/filter_5.html`




















