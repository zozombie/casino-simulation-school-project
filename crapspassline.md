import random
random.seed(3456)

betted_amounts = (48, 67, 10)
sim_win = 0
sim_lose = 0
gain_player = 0
gain_casino = 0

class Craps(object):
    def __init__(self, minimum_bet):
        self.minimum_bet = minimum_bet

    def AboveMinimum(self, betted_amounts):
        result_Above = []

        for betted_amount in betted_amounts:
            if betted_amount >= self.minimum_bet:
                print "New game. Your bet is", betted_amount
                result_Above.append(1)
            else:
                print "Sorry, you can't play since you don't have enough money! See you next time!"
                result_Above.append(0)
        return result_Above

    def Dices(self):
        d1 = random.randrange(1, 6)
        d2 = random.randrange(1, 6)
        sum = d1 + d2
        print "You rolled", d1, "+", d2, "=", sum
        return sum

    def SimulationGame(self, betted_amounts):
        result_bets = []

        global sim_win, sim_lose, point
        global gain_casino, gain_player

        for betted_amount in betted_amounts:
            if betted_amount <= self.minimum_bet:
                print("you don't have enough money!")
                return False
            else:
                print "New game. Your bet is: ", betted_amount

                # for the first roll
                d = self.Dices()
                if d in (7, 11):
                    gameStatus = "WON"
                    result_bets.append(1)
                    print "Congratz! You Won!"
                elif d in (2, 3, 12):
                    gameStatus = "LOST"
                    result_bets.append(0)
                    print "Oops! You lost!"
                else:
                    gameStatus = "CONTINUE"
                    point = d
                    print "Your point is", "[", point, "]", "Game continues"

                    # for subsequence rolls
                while gameStatus == "CONTINUE":
                    sum = self.Dices()
                    if not sum in (7, point):
                        print "Please roll again!"
                    elif sum == point:
                        gameStatus = "WON"
                        result_bets.append(1)
                        print "You made your point!"
                    elif sum == 7:
                        gameStatus = "LOST"
                        result_bets.append(0)
                        print "You crapped out!"

                if gameStatus == "WON":
                    sim_win += 1
                    gain_player += betted_amount
                else:
                    sim_lose += 1
                    gain_casino += betted_amount
                    print "There are", sim_win, "correct bet(s) and", sim_lose, "incorrect bets."

                    if sim_win > 0:
                       print "The total gain_player", gain_player
                    if sim_lose >0:
                       print "the total amount won by the casino", gain_casino - gain_player

                    return result_bets

    def RollTheDices(self):
        total = sim_win + sim_lose
        percent = float(sim_win) / total * 100
        print "There are", total, "players " 
        print "Won ", sim_win
        print "Lost ", sim_lose
        print "Overall, you have %d%% to win!" % percent

c = Craps(20)

c.AboveMinimum(betted_amounts)
c.Dices()
c.SimulationGame(betted_amounts)
c.RollTheDices()
