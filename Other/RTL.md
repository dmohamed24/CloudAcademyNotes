# 🚀 **React Testing Library Cheatsheet**

## 🎯 **Common Queries**

### 🔍 **By Role** (preferred)

`screen.getByRole("button", { name: /submit/i }); screen.findByRole("alert"); screen.queryByRole("dialog");`

### 🔤 **By Text**

`screen.getByText("Hello world"); screen.getByText(/hello/i);`

### 📝 **By Label / Placeholder**

`screen.getByLabelText("Email"); screen.getByPlaceholderText("Enter password");`

### 🏷️ **By Test ID** (avoid unless necessary)

`screen.getByTestId("login-button");`

### 🧩 **Query variants**

|Query|Behavior|
|---|---|
|`getBy*`|Throws error if not found (sync)|
|`queryBy*`|Returns `null` if not found (sync)|
|`findBy*`|Returns a Promise; waits for element (async)|

---

# ⌨️ **userEvent Usage**

### Typing

`await userEvent.type(input, "hello");`

### Clicking

`await userEvent.click(button);`

### Selecting

`await userEvent.selectOptions(selectElement, "option-value");`

### Clearing input

`await userEvent.clear(input);`

---

# 🔄 **Testing Component Behavior**

### Testing Conditional Rendering

`expect(screen.queryByText("Error")).not.toBeInTheDocument();  await userEvent.click(button);  expect(await screen.findByText("Error")).toBeInTheDocument();`

### Testing Props Callback

`const mock = vi.fn(); render(<MyComponent onSubmit={mock} />);  await userEvent.click(button); expect(mock).toHaveBeenCalled();`

---

# 🧪 **Mocking**

### Mocking a function prop

`const mockFn = vi.fn();`

### Mocking modules

`vi.mock("axios");`

### Reset mocks

`vi.clearAllMocks(); vi.resetAllMocks();`

---

# 🧹 **Cleanup**

Usually automatic, but sometimes useful:

`cleanup();`

---

# 📝 **Most Common Jest + Testing Library Assertions**

Below is a comprehensive list of _the most frequently used assertions_.

---

## 📌 **DOM Assertions** (from `@testing-library/jest-dom`)

### 🔹 Visibility & existence

`expect(element).toBeInTheDocument(); expect(element).not.toBeInTheDocument();  expect(element).toBeVisible(); expect(element).not.toBeVisible();`

### 🔹 Text content

`expect(element).toHaveTextContent("Hello"); expect(element).toHaveTextContent(/hello/i); expect(element).not.toHaveTextContent("Error");`

### 🔹 Form element assertions

`expect(button).toBeDisabled(); expect(button).toBeEnabled();  expect(input).toHaveValue("hello"); expect(input).toHaveDisplayValue("hello");`

### 🔹 Class / style

`expect(element).toHaveClass("active"); expect(element).not.toHaveClass("hidden");  expect(element).toHaveStyle({ color: "red" });`

### 🔹 Attributes

`expect(link).toHaveAttribute("href", "/home"); expect(img).toHaveAttribute("src");`

### 🔹 Checked / selected

`expect(checkbox).toBeChecked(); expect(checkbox).not.toBeChecked();  expect(option).toBeSelected();`

---

## 📌 **Mock Function Assertions** (Jest / Vitest)

### 🔹 Call count

`expect(mockFn).toHaveBeenCalled(); expect(mockFn).toHaveBeenCalledTimes(1);`

### 🔹 Call args

`expect(mockFn).toHaveBeenCalledWith("hello", 123); expect(mockFn).toHaveBeenLastCalledWith("final");`

### 🔹 Returned values

`expect(mockFn).toHaveReturned(); expect(mockFn).toHaveReturnedWith("result");`

---

## 📌 **General Jest Assertions**

### Booleans

`expect(true).toBe(true); expect(value).toBeFalsy(); expect(value).toBeTruthy();`

### Equality

`expect(value).toBe(10); expect(obj).toEqual({ id: 1 }); expect(obj).toStrictEqual({ id: 1 });`

### Numbers

`expect(num).toBeGreaterThan(5); expect(num).toBeLessThanOrEqual(10);`

### Arrays

`expect(array).toContain("item");`

### Errors

`expect(() => fn()).toThrow(); expect(() => fn()).toThrow("message");`

---

# 🎁 Bonus: **Suggested RTL Testing Flow**

```
render(component);

// 1. Select elements
const input = screen.getByLabelText("Email");
const button = screen.getByRole("button", { name: "Login" });

// 2. User interacts
await userEvent.type(input, "test@test.com");
await userEvent.click(button);

// 3. Assert output
expect(mockSubmit).toHaveBeenCalledWith(...data);
expect(await screen.findByRole("alert")).toHaveTextContent("Error");

```

---

# ✅ Want this as a downloadable PDF or markdown file?