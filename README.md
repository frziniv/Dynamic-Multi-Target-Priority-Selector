# Dynamic-Multi-Target-Priority-Selector
# PrioritySelector.py

class PrioritySelector:
    def __init__(self):
        # Weights for different factors
        self.DISTANCE_WEIGHT = 0.4
        self.HP_WEIGHT = 0.6

    def calculate_threat_score(self, distance: float, hp: float) -> float:
        """
        Calculates a score where a HIGHER score means a HIGHER priority target.
        Priority increases if the enemy is close AND has low HP (easy kill).
        """
        # Inverse distance (closer is higher threat)
        distance_factor = 1.0 / (distance + 1.0) 
        # Inverse HP (lower HP is higher priority for elimination)
        hp_factor = 1.0 / (hp + 1.0)
        
        return (distance_factor * self.DISTANCE_WEIGHT) + (hp_factor * self.HP_WEIGHT)

    def get_best_target(self, enemies: list[dict]) -> dict:
        """
        Input: List of enemies with 'id', 'distance', and 'hp'.
        Output: The enemy object with the highest priority score.
        """
        best_target = None
        max_score = -1.0

        for enemy in enemies:
            score = self.calculate_threat_score(enemy['distance'], enemy['hp'])
            if score > max_score:
                max_score = score
                best_target = enemy
        
        return best_target

# Example Usage
if __name__ == "__main__":
    selector = PrioritySelector()
    enemy_list = [
        {"id": "Enemy_A", "distance": 50, "hp": 10},  // Low HP but far
        {"id": "Enemy_B", "distance": 10, "hp": 100} // Close but full HP
    ]
    target = selector.get_best_target(enemy_list)
    print(f"Targeting: {target['id']} based on optimal threat/elimination ratio.")
