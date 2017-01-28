import random

random.seed(3456)

sim_win = 0
sim_lose = 0

bankroll1 = []
bankroll2 = []
cash_casino1 = []
cash_casino2 = []


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

    def RollTheDices(self, bets):
        result_guess = []
        global sim_win, sim_lose
        for bet in bets:
            if bet < 2 or bet > 12:
                print ValueError("Please give a bet between 2 and 12")
            else:
                 print "Your number is", bet

        lucky_number = self.Dices()

        for bet in bets:
            if bet == lucky_number:
                result_guess.append(1)
                sim_win += 1
            else:
                result_guess.append(0)
                sim_lose += 1

        if sim_win > 0:
            print "There are", sim_win, "correct bet(s) and", sim_lose, "incorrect bets."
        else:
            print "No winners this round. All of the {} bets are incorrect".format(sim_lose)

            return lucky_number, result_guess

    def SimulateGame(self, bets, betted_amounts):
        result_player =[]
        lucky_number = self.Dices()
        global bankroll1, bankroll2
        global cash_casino1, cash_casino2


        for betted_amount in betted_amounts:
            if betted_amount <= self.minimum_bet:
                print ValueError("you do not have enough money!")

            else:
                print "New game. Your bet is", betted_amount

                for bet in bets:
                    if bet == lucky_number:
                        if bet in (4, 5, 6, 7, 8, 9):
                            bankroll1 += betted_amount + betted_amount
                            result_player.append(1)
                            print bankroll1


                        else:
                            bankroll2 += betted_amount + betted_amount + betted_amount
                            result_player.append(1)
                            print bankroll2

                    else:
                        if bet in (4, 5, 6, 7, 8, 9):
                            cash_casino1 += betted_amount + betted_amount
                            result_player.append(0)
                            print cash_casino1

                        else:
                            cash_casino2 += betted_amounts + betted_amount + betted_amount
                            result_player.append(0)
                            print cash_casino2


c = Craps(20)

betted_amounts = [65, 85, 10]
#c.AboveMinimum(betted_amounts)
bets = [7, 4, 12]
#c.RollTheDices(bets)


c.SimulateGame(bets,betted_amounts)
