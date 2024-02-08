# DMB_PROJECT

Réalisé par Quentin LEGRAND et Bastien SAUVAT

## 💻 0. Prérequis

Pour lancer le Projet, ajoutez le fichiers **"./kill_match_stats_final_0.csv" et "./agg_match_stats_0.csv"** à la racine du projet.

Si vous décidez de lancer le script python, plutôt que le notebook, il est possible que certaines librairies soient à installer.

## 💻 1. Présentation du Projet

Ce projet porte sur le prétraitement et l'exploration de l'analyse des données du SARS-Cov-2 à l'échelle mondiale à l'aide de PySpark. Le jeu de données a été créé et est maintenu par Our World in Data (OWiD).

Avant de commencer l'analyse du dataset, nous allons effectuer certains nettoyages du dataset. Nous allons notamenent resteindre les dates du dataset à janvier et février 2021 pour réduire le temps d'éxecution. 

Durant cette analyse nous allons étudier le nombre de cas recensé, de nouveau test, l'évolution des taux de mortalité, l'évolution du nombre de patients admis en soins intensifs, la corélation géographique de l'excès de mortalité, la corélation entre certains paramètres et la realtion entre le covid et l'état de santé.

## 💻 2. Visualisation du jeu de données

Commençons par visualiser le dataset :

```markdown
| iso_code | continent | location    | date       | total_cases | new_cases | new_cases_smoothed | total_deaths | new_deaths | new_deaths_smoothed | total_cases_per_million | new_cases_per_million | new_cases_smoothed_per_million | total_deaths_per_million | new_deaths_per_million | new_deaths_smoothed_per_million | reproduction_rate | icu_patients | icu_patients_per_million | hosp_patients | hosp_patients_per_million | weekly_icu_admissions | weekly_icu_admissions_per_million | weekly_hosp_admissions | weekly_hosp_admissions_per_million | total_tests | new_tests | total_tests_per_thousand | new_tests_per_thousand | new_tests_smoothed | new_tests_smoothed_per_thousand | positive_rate | tests_per_case | tests_units | total_vaccinations | people_vaccinated | people_fully_vaccinated | total_boosters | new_vaccinations | new_vaccinations_smoothed | total_vaccinations_per_hundred | people_vaccinated_per_hundred | people_fully_vaccinated_per_hundred | total_boosters_per_hundred | new_vaccinations_smoothed_per_million | new_people_vaccinated_smoothed | new_people_vaccinated_smoothed_per_hundred | stringency_index | population | population_density | median_age | aged_65_older | aged_70_older | gdp_per_capita | extreme_poverty | cardiovasc_death_rate | diabetes_prevalence | female_smokers | male_smokers | handwashing_facilities | hospital_beds_per_thousand | life_expectancy | human_development_index | excess_mortality_cumulative_absolute | excess_mortality_cumulative | excess_mortality | excess_mortality_cumulative_per_million |
|----------|-----------|-------------|------------|-------------|-----------|--------------------|--------------|------------|---------------------|--------------------------|-----------------------|---------------------------------|--------------------------|------------------------|-----------------------------------|-------------------|--------------|--------------------------|---------------|---------------------------|-----------------------|---------------------------------|------------------------|------------------------------------|-------------|-----------|-------------------------|-------------------------|--------------------|--------------------------------|----------------|----------------|-------------|--------------------|-------------------|------------------------|-----------------|------------------+-------------|--------------------|------------------|--------------------------|----------------|-----------------|------------|---------------------|-------------------|-------------------------|--------------------------|-----------------------------|-----------------------------------|-----------------------|-------------------------------------------|-------------------|------------|--------------------|------------|---------------|---------------|----------------|------------------|---------------------|---------------------|----------------|--------------|-----------------------|---------------------------|----------------|------------------------|---------------------------------|--------------------------|----------------|---------------------------------------|
| AFG      | Asia      | Afghanistan | 2020-02-24 | 5.0         | 5.0       | NULL               | NULL         | NULL       | NULL                | 0.126                    | 0.126                 | NULL                            | NULL                     | NULL                   | NULL                              | NULL              | NULL         | NULL                     | NULL          | NULL                      | NULL                  | NULL                            | NULL                     | NULL                               | NULL        | NULL      | NULL                    | NULL                    | NULL               | NULL                           | NULL           | NULL            | NULL        | NULL               | NULL              | NULL                     | NULL            | NULL            | NULL       | NULL               | NULL              | NULL                    | NULL           | NULL              | NULL                     | NULL                        | NULL                        | NULL                              | NULL                       | NULL                                      | NULL                  | NULL       | NULL               | NULL       | NULL          | NULL          | NULL           | NULL             | NULL                | NULL                | NULL           | NULL         | NULL                  | NULL                      | NULL           | NULL                   | NULL                              | NULL                       | NULL           | NULL                                  |
| AFG      | Asia      | Afghanistan | 2020-02-25 | 5.0         | 0.0       | NULL               | NULL         | NULL       | NULL                | 0.126                    | 0.0                   | NULL                            | NULL                     | NULL                   | NULL                              | NULL              | NULL         | NULL                     | NULL          | NULL                      | NULL                  | NULL                            | NULL                     | NULL                               | NULL        | NULL      | NULL                    | NULL                    | NULL               | NULL                           | NULL           | NULL            | NULL        | NULL               | NULL              | NULL                     | NULL            | NULL            | NULL       | NULL               | NULL              | NULL                    | NULL           | NULL              | NULL                     | NULL                        | NULL                        | NULL                              | NULL                       | NULL                                      | NULL                  | NULL       | NULL               | NULL       | NULL          | NULL          | NULL           | NULL             | NULL                | NULL                | NULL           | NULL         | NULL                  | NULL                      | NULL           | NULL                   | NULL                              | NULL                       | NULL           | NULL                                  |
| AFG      | Asia      | Afghanistan | 2020-02-26 | 5.0         | 0.0       | NULL               | NULL         | NULL       | NULL                | 0.126                    | 0.0                   | NULL                            | NULL                     | NULL                   | NULL                              | NULL              | NULL         | NULL                     | NULL          | NULL                      | NULL                  | NULL                            | NULL                     | NULL                               | NULL        | NULL      | NULL                    | NULL                    | NULL               | NULL                           | NULL           | NULL            | NULL        | NULL               | NULL              | NULL                    | NULL           | NULL              | NULL                     | NULL                        | NULL                        | NULL                              | NULL                       | NULL                                      | NULL                  | NULL       | NULL               | NULL       | NULL          | NULL          | NULL           | NULL             | NULL                | NULL                | NULL           | NULL         | NULL                  | NULL                      | NULL           | NULL                   | NULL                              | NULL                       | NULL           | NULL                                  |
| AFG      | Asia      | Afghanistan | 2020-02-27 | 5.0         | 0.0       | NULL               | NULL         | NULL       | NULL                | 0.126                    | 0.0                   | NULL                            | NULL                     | NULL                   | NULL                              | NULL              | NULL         | NULL                     | NULL          | NULL                      | NULL                  | NULL                            | NULL                     | NULL                               | NULL        | NULL      | NULL                    | NULL                    | NULL               | NULL                           | NULL           | NULL            | NULL        | NULL               | NULL              | NULL                    | NULL           | NULL              | NULL                     | NULL                        | NULL                        | NULL                              | NULL                       | NULL                                      | NULL                  | NULL       | NULL               | NULL       | NULL          | NULL          | NULL           | NULL             | NULL                | NULL                | NULL           | NULL         | NULL                  | NULL                      | NULL           | NULL                   | NULL                              | NULL                       | NULL           | NULL                                  |
| AFG      | Asia      | Afghanistan | 2020-02-28 | 5.0         | 0.0       | NULL               | NULL         | NULL       | NULL                | 0.126                    | 0.0                   | NULL                            | NULL                     | NULL                   | NULL                              | NULL              | NULL         | NULL                     | NULL          | NULL                      | NULL                  | NULL                            | NULL                     | NULL                               | NULL        | NULL      | NULL                    | NULL                    | NULL               | NULL                           | NULL           | NULL            | NULL        | NULL               | NULL              | NULL                    | NULL           | NULL              | NULL                     | NULL                        | NULL                        | NULL                              | NULL                       | NULL                                      | NULL                  | NULL       | NULL               | NULL       | NULL          | NULL          | NULL           | NULL             | NULL                | NULL                | NULL           | NULL         | NULL                  | NULL                      | NULL           | NULL                   | NULL                              | NULL                       | NULL           | NULL                                  |
+----------+-----------+-------------+------------+-------------+-----------+--------------------+--------------+------------+---------------------+--------------------------+-----------------------+---------------------------------+--------------------------+----------------------+-------------------------------+-------------------+--------------+--------------------------+---------------+---------------------------+-----------------------+---------------------------------+------------------------+------------------------------------+-------------+-----------+-------------------------+-------------------------+--------------------+--------------------------------+----------------+----------------+-------------+--------------------+-------------------+------------------------+-----------------+------------------+-------------+--------------------+------------------+--------------------------+--------------------------+-----------------------------+-----------------------------------+-----------------------+-------------------------------------------+-------------------+------------+--------------------+------------+---------------+---------------+----------------+------------------+-...
```

## 💻 3. Les meilleurs joueurs

Voici un échantillon de statistiques de joueurs :

```markdown
| Killer Name       | Number of Games  | Average Kills per Game |
|-------------------|------------------|------------------------|
| KrazyPortuguese   | 1.00             | 1.00                   |
| nide2Bxiaojiejie  | 1.00             | 3.00                   |
| Ascholes          | 1.00             | 2.00                   |
| Weirdo7777        | 1.00             | 1.00                   |
| Solayuki1         | 1.00             | 1.00                   |
| xuezhiqian717     | 1.00             | 3.00                   |
| pdfjkkvjk         | 1.00             | 1.00                   |
| xiaogao13         | 1.00             | 2.00                   |
| Jingchita         | 1.00             | 1.00                   |
| Alexande-999      | 1.00             | 3.00                   |
```


Voici le TOP 10 des meilleurs joueurs en moyenne de kills : 

```markdown
| Killer Name       | Number of Games  | Average Kills per Game |
|-------------------|------------------|------------------------|
| gogolnyg          | 1.00             | 62.00                  |
| WG-Qun326373092-  | 1.00             | 61.00                  |
| Wgqun373692007_   | 1.00             | 61.00                  |
| QQqun-608179539   | 1.00             | 60.00                  |
| WG_Qun-326373092  | 1.00             | 60.00                  |
| WgQun_373692007   | 1.00             | 55.00                  |
| Yaraz-1           | 1.00             | 54.00                  |
| WG_Qun_656356507  | 1.00             | 54.00                  |
| 2Vx-shenxian852   | 1.00             | 53.00                  |
| yy555333_         | 1.00             | 53.00                  |
```

Voici le TOP 10 des meilleurs joueurs en moyenne de placement par partie :

```markdown
| Player Name       | Number of Games  | Average placement per Game |
|-------------------|------------------|----------------------------|
| Rywito            | 1.00             | 1.00                       |
| XFHeiGod          | 1.00             | 1.00                       |
| vt666             | 1.00             | 1.00                       |
| fszxc             | 1.00             | 1.00                       |
| youhunzhanshi     | 1.00             | 1.00                       |
| fisnysduo7721     | 1.00             | 1.00                       |
| Tzrtao            | 1.00             | 1.00                       |
| Tibetan1998       | 1.00             | 1.00                       |
| The_Real_Slim     | 1.00             | 1.00                       |
| sjj5655           | 1.00             | 1.00                       |
```

Voici le TOP 10 des meilleurs joueurs en moyenne de kills et ayant joué plus de 4 parties: 

```markdown
| Killer Name       | Number of Games  | Average Kills per Game |
|-------------------|------------------|------------------------|
| negronegro        | 5.00             | 32.60                  |
| labowoo           | 4.00             | 32.00                  |
| z1148139722       | 5.00             | 27.60                  |
| DBC-d             | 7.00             | 26.71                  |
| Yy_5908_xlaojun   | 5.00             | 25.00                  |
| WG-Qqun185121200  | 4.00             | 24.25                  |
| yema-301323278    | 5.00             | 24.00                  |
| BUG__Xsz          | 4.00             | 23.75                  |
| ChuZhiZhenHuiWan  | 5.00             | 23.20                  |
| tongnmgg          | 4.00             | 23.00                  |
```

Voici le TOP 10 des meilleurs joueurs en moyenne de placement et ayant joué plus de 4 parties: 

```markdown
| Player Name        | Number of Games  | Average placement per Game |
|--------------------|------------------|----------------------------|
| dazhong66          | 4.00             | 1.00                       |
| Georgedesne        | 4.00             | 1.00                       |
| GH-gohome          | 5.00             | 1.00                       |
| prettyACebb____    | 4.00             | 1.00                       |
| FUMMMMMM           | 4.00             | 1.00                       |
| qq1020583302       | 6.00             | 1.00                       |
| Q1nzheng           | 4.00             | 1.00                       |
| QvnWG-10100166     | 4.00             | 1.00                       |
| IIIIIII1IIIIIIII   | 4.00             | 1.00                       |
| tongnmgg           | 4.00             | 1.00                       |
```

Etude d'un joueur en particulier : 

```markdown
1. Killer Name: gogolnyg, Number of Games: 1.00, Average Kills per Game: 62.00
```

```markdown
1. player Name: gogolnyg, Number of Games: 1.00, Average placement per Game: 1.00
```

En conclusion, à première vue il ne semble pas y avoir de rapport entre l’objectif d’être le dernier en vie et la nécessité d’éliminer un maximum de concurrents. 

## 💻 4. Score des joueurs

Voici le TOP 10 des meilleurs joueurs selon le classement combiné : 

```markdown
| Player            | Score    |
|-------------------|----------|
| labowoo           | 13683.00 |
| labowoo           | 13363.00 |
| gogolnyg          | 13272.00 |
| MacOSX            | 13236.00 |
| Wgqun373692007_   | 13091.00 |
| qcsicknasty       | 13023.00 |
| WG-Qun326373092-  | 12883.00 |
| WG_Qun-326373092  | 12623.00 |
| QQqun-608179539   | 12201.00 |
| 6l8-I34-8O0-g9un  | 11618.00 |
```

Avec ce format de classement, il semblerait que les joueurs ayant fait le plus de kill soient les plus avantagés. En effet, on retoruve 5 joueurs du TOP 10 des kills dans ce TOP : Wgqun373692007_, WG-Qun326373092-, WG_Qun-326373092, QQqun-608179539, gogolnyg. 

De plus, le TOP 1 du classement labowoo, qui occupe la 1ère et 2ème place du classement, est un aussi un joueur de kill puisque qu'on le retrouve 2ème du TOP 10 des kills avec plus de 4 partie jouées.