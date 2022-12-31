"""Створити базовий клас СПОРТИВНА УНІВЕРСІАДА (задаються назва і дата проведення).
Створити похідний клас ЗМАГАННЯ МІЖ 2 КОМАНДАМИ (задаються дані про двох суперників, кількість очок (голів),
набраних кожним з них проти іншого). Для введених даних про різні змагання визначити загальну кількість перемог
команд кожного університету і відсортувати університети за зростанням цієї кількості"""

import datetime as dt


class universiada:
    def __init__(self, name, date):
        self.name = name
        self.date = date

    def __str__(self):
        return f"{self.name} - {self.date}"


class competition(universiada):
    def __init__(self, type, univ1, score1, univ2, score2, name, date):
        super().__init__(name, date)
        self.type = type
        self.univ1 = univ1
        self.score1 = score1
        self.univ2 = univ2
        self.score2 = score2
        self.winner = self.winnerandloser()[0]
        self.loser = self.winnerandloser()[1]

    def winnerandloser(self):
        if self.score1 > self.score2:
            return [self.univ1, self.univ2]
        elif self.score2 > self.score1:
            return [self.univ2, self.univ1]
        else:
            return ["draw", "draw"]

    def __str__(self):
        return f"\n{self.univ1} {self.score1} - {self.score2} {self.univ2}\nWinner: {self.winner}"

    def competition_info(self):
        return super().__str__()


def sort(arr, univ):
    teams = []
    scores = []
    for i in arr:
        if i.name == univ:
            if i.winner == "draw":
                if i.univ2 not in teams:
                    teams.append(i.univ2)
                    scores.append(0)
                if i.univ1 not in teams:
                    teams.append(i.univ1)
                    scores.append(0)
                    continue
            else:
                if i.loser not in teams:
                    teams.append(i.loser)
                    scores.append(0)
                if i.winner not in teams:
                    teams.append(i.winner)
                    scores.append(1)
                    continue
                else:
                    scores[teams.index(i.winner)] += 1
                    continue

    for i in range(len(teams)):
        for j in range(len(teams)-1):
            if scores[j] < scores[j+1]:
                scores[j], scores[j + 1] = scores[j + 1], scores[j]
                teams[j], teams[j + 1] = teams[j + 1], teams[j]
    print(f"\nРезультати {univ}:")
    for i in range(0, len(scores)):
        print(f"№{i+1}. {teams[i]} - {scores[i]}")


def main():
    univ = universiada("NULP Camp 2022", dt.date(2022, 12, 12))
    univ1 = universiada("Kyiv League 2022", dt.date(2022, 12, 12))
    comp1 = competition("Football", "NULP", 5, "LNU", 0, univ.name, univ.date)
    comp2 = competition("Football", "TNTU", 2, "LNU", 1, univ.name, univ.date)
    comp3 = competition("Football", "NULP", 2, "TNTU", 3, univ.name, univ.date)
    comp4 = competition("Football", "KPI", 3, "TNTU", 5, univ.name, univ.date)
    comp5 = competition("Football", "KPI", 2, "LNU", 1, univ.name, univ.date)
    comp6 = competition("Football", "NULP", 2, "KPI", 2, univ.name, univ.date)
    comp7 = competition("Football", "KNU", 3, "KPI", 2, univ1.name, univ1.date)
    comp8 = competition("Football", "KNU", 3, "KPI", 3, univ1.name, univ1.date)
    comp9 = competition("Football", "KNU", 3, "KPI", 4, univ1.name, univ1.date)
    arr = [comp1, comp2, comp3, comp4, comp5, comp6, comp7, comp8, comp9]
    for i in arr:
        print(i.competition_info())
        print(i)
    sort(arr, "NULP Camp 2022")
    sort(arr, "Kyiv League 2022")


if "__main__":
    main()