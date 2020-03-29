---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---





### Simulations 

bla bla bla



#### Simple

Simple simulation. 300 people, propability of infection is 100% same is propability of that infection will convert to sickness. Fatality ratio is 2% regardless of age. 

{% raw %}
<iframe src="https://qunabu.github.io/covid-simulation/?width=1024&height=509&amount=300&probInfection=1&probInfectionSick=1&fatalAge9=0.002&fatalAge19=0.002&fatalAge29=0.002&fatalAge39=0.002&fatalAge49=0.002&fatalAge59=0.002&fatalAge69=0.002&fatalAge79=0.002&fatalAge100=0.002&quarantineWalls=0&quarantineNotMove=0&iframe=1" width="1040" height="649"></iframe>
{% endraw %}





#### Age structure 

This simulation has a different fatality ratio based on the age. Age structure is normalized 

| Age      | 0-9 | 10-19 | 20-29 | 30-39 | 40-49 | 50-59 | 60-69 | 70-79 | 80+ |
|----------|-----|-------|-------|-------|-------|-------|-------|-------|-----|
| Stucture | 3%  | 7%    | 14%   | 19%   | 20%   | 19%   | 13%   | 3%    | 2%  |

0-9 3% means that from the whole population 3% has age between 0 to 9 (including 9) and so on. The reason to have age in the simulatio is that fatality ratio is different for each range. All the percenage is sum must eqals 100% 

| Age      | 0-9    | 10-19  | 20-29 | 30-39 | 40-49 | 50-59 | 60-69 | 70-79 | 80+  |
|----------|--------|--------|-------|-------|-------|-------|-------|-------|------|
| Stucture | 0.002% | 0.006% | 0.03% | 0.08% | 0.15% | 0.6%  | 2.2%  | 5.1%  | 9.3% |

0-9 3% 0.002% means that person with age in that range has 0.002% change of fatal sickenes, person aged more the 80 has 9.3% change of fatal sickenes.

{% raw %}
<iframe src="https://qunabu.github.io/covid-simulation/?width=1024&height=509&amount=289&quarantineWalls=0&quarantineNotMove=0&iframe=1" width="1040" height="649"></iframe>
{% endraw %}




