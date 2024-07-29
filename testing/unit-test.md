# 단위 테스트(Unit Test)

## **단위 테스트(Unit Test) 란?** <a href="#what-is-unit-test" id="what-is-unit-test"></a>

단위 테스트는 하나의특정 모듈이 의도된 대로 정확히 작동하는지 검증하는 절차를 말합니다.\
스프링에서는 단위 테스트를 위해  [**Mock Object**](test-library/mock-objects.md#mock-object)를 사용하여 코드를 독립적으로 테스트 할 수 있고\
Mock Object를 편하게 활용하기 위해 스프링에서 [**Mockito**](test-library/mock-objects.md#mockito)를 활용할 수 있습니다.



## **스프링 단위 테스트**

스프링에서는 **JUnit**과 **Mockito**를 활용하여 편리한 단위 테스트를 수행 할 수 있습니다.\
\
다음 예제는 JUnit5와 Mockito를 활용한 스프링 단위 테스트 입니다.\
우리는 장바구니에 클라이언트가 선택한 음식을 담는 코드 단위 테스트를 수행할 것 입니다.

{% code title="장바구니 인터페이스" %}
```java
public interface CartService {
    void addItemToCart(Long clientId, Long foodId);
}
```
{% endcode %}

{% code title="장바구니 서비스" %}
```java
import org.springframework.stereotype.Service;

@Service
public class CartServiceImpl implements CartService {

    private final CartRepository cartRepository;
    private final FoodRepository foodRepository;

    public CartServiceImpl(CartRepository cartRepository, FoodRepository foodRepository) {
        this.cartRepository = cartRepository;
        this.foodRepository = foodRepository;
    }

    @Override
    public void addItemToCart(Long clientId, Long foodId) {
        Cart cart = cartRepository.findByClientId(clientId)
                .orElse(new Cart(clientId));
        Food food = foodRepository.findById(foodId)
                .orElseThrow(() -> new IllegalArgumentException("Food not found"));
        
        cart.addFood(food);
        cartRepository.save(cart);
    }
}

```
{% endcode %}

먼저 간단한 **장바구니 인터페이스**와 장바구니 인터페이스를 구현한 **장바구니 서비스가** 있습니다.\


{% code title="장바구니 엔티티" %}
```java
import javax.persistence.*;
import java.util.HashSet;
import java.util.Set;

@Entity
public class Cart {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private Long clientId;

    @ManyToMany
    private Set<Food> foods = new HashSet<>();

    public Cart(Long clientId) {
        this.clientId = clientId;
    }

    public void addFood(Food food) {
        foods.add(food);
    }
}

```
{% endcode %}

{% code title="음식 엔티티" %}
```java
import javax.persistence.*;

@Entity
public class Food {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
}

```
{% endcode %}

{% code title="카트 서비스 단위  테스트" %}
```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;

import java.util.Optional;

import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
public class CartServiceTest {

    @Mock
    private CartRepository cartRepository;

    @Mock
    private FoodRepository foodRepository;

    @InjectMocks
    private CartServiceImpl cartService;

    private Cart cart;
    private Food food;

    @BeforeEach
    void setUp() {
        cart = new Cart(1L);
        food = new Food();
        food.setId(1L);
        food.setName("Pizza");
    }

    @Test
    void addItemToCart() {
        when(cartRepository.findByClientId(any(Long.class))).thenReturn(Optional.of(cart));
        when(foodRepository.findById(any(Long.class))).thenReturn(Optional.of(food));
        doNothing().when(cartRepository).save(any(Cart.class));

        cartService.addItemToCart(1L, 1L);

        verify(cartRepository, times(1)).findByClientId(any(Long.class));
        verify(foodRepository, times(1)).findById(any(Long.class));
        verify(cartRepository, times(1)).save(any(Cart.class));
        assert(cart.getFoods().contains(food));
    }
}

```
{% endcode %}
