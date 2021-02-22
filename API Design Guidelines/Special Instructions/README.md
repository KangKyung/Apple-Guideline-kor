# Special Instructions

* **Label tuple members and name closure parameters** 
<br>test

    test

    ```swift
    /// Ensure that we hold uniquely-referenced storage for at least
    /// `requestedCapacity` elements.
    ///
    /// If more storage is needed, `allocate` is called with
    /// `byteCount` equal to the number of maximally-aligned
    /// bytes to allocate.
    ///
    /// - Returns:
    ///   - reallocated: `true` iff a new block of memory
    ///     was allocated.
    ///   - capacityChanged: `true` iff `capacity` was updated.
    mutating func ensureUniqueStorage(
        minimumCapacity requestedCapacity: Int, 
        allocate: (_ byteCount: Int) -> UnsafePointer<Void>
    ) -> (reallocated: Bool, capacityChanged: Bool)
    ```

    test

* **Take extra care with unconstrained polymorphism** 
<br>test
<br>(ex> `Any`, `AnyObject`, and unconstrained generic parameters)

    test

    ```swift
    ❌

    struct Array {
        /// Inserts `newElement` at `self.endIndex`.
        public mutating func append(_ newElement: Element)

        /// Inserts the contents of `newElements`, in order, at
        /// `self.endIndex`.
        public mutating func append(_ newElements: S)
            where S.Generator.Element == Element
    }
    ```

    test

    ```swift
    ❌

    var values: [Any] = [1, "a"]
    values.append([2, 3, 4]) // [1, "a", [2, 3, 4]] or [1, "a", 2, 3, 4]?
    ```

    test

    ```swift
    ✅

    struct Array {
        /// Inserts `newElement` at `self.endIndex`.
        public mutating func append(_ newElement: Element)

        /// Inserts the contents of `newElements`, in order, at
        /// `self.endIndex`.
        public mutating func append(contentsOf newElements: S)
            where S.Generator.Element == Element
    }
    ```

    test